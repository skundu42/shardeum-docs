---
title: Node Lifecycle
sidebar_position: 2
image: /img/new/i31.jpg
---


# Node Lifecycle

### 1. Overview

The lifecycle of nodes within the Shardeum network is meticulously designed to ensure robust security, efficient operation, and seamless scalability. Nodes transition through various states, beginning as standby nodes and potentially advancing to active nodes, participating fully in network operations.

### 2. Node States

In the network, nodes exist in two primary states:

* **Standby Nodes**: Nodes here are not yet holding or processing data. Their primary role is to enhance network security by participating indirectly. This state serves as a buffer against potential attacks, such as 51% attacks, as attackers would need to control a significant percentage of both active and standby nodes to succeed.
* **Active Nodes**: These nodes are fully participating in the network operations, including processing transactions and storing data. They are essential for maintaining the network's operational integrity and executing consensus.

### 3. Node Registration and Rotation

* **Staking and Joining**: To enter the standby list, nodes must first stake and submit a join request. This process involves creating a staking transaction, which is validated and recorded on the blockchain. The staking information is stored in specific data structures designed for this purpose. Each staking transaction is associated with a unique address and includes a JSON blob containing the staking instructions, encoded in base 64. This allows the system to easily recognize and validate staking transactions without processing them through the EVM. Once validated, the user's balance is updated, and a staking record is created.\` This process is secured by requiring nodes to present signed certificates, ensuring that only verified nodes can enter the queue. The request to join the standby list can be seen at [`addStandbyJoinRequests()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Join/v2/index.ts#L107).
* **Rotation:** Nodes are periodically rotated out of active duty as a measure of security and efficiency. This rotation follows a deterministic schedule, where typically one node is rotated out per cycle after completing its duties, and another is brought in from the standby list. The expired and removed nodes are returned from [`getExpiredRemoved()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Rotation.ts#L118).

![Cycle](/img/new/i8.jpg)

### 4. Node Selection and Activation

* **Deterministic Random Selection**: The network employs a deterministic algorithm to select nodes from the standby list. This selection method prevents manipulation and maintains a smooth transition from standby to active states. This selection not only picks the node but also assigns it an ID, which determines the address space it will cover and who its neighbors will be. Active validators create consensus on the list’s integrity by hashing it each cycle. This consensus prevents any single node from manipulating its chance of selection and subsequently landing on the same shard by generating new public-private key pairs in bulk. The selection process employs 'future numbers,' which means that the actual random numbers determining which nodes are selected and their placement are not generated until later in the selection phase. This setup creates virtually impossible conditions for any node to exploit the system and prevents potential attacks where one might control 51% of the consensus, thereby harming targeted accounts. The selection process is executed in [`selectNodes()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Join/v2/select.ts#L47).
* **Syncing Process:** When a node is selected to become active, it first enters a syncing mode to prevent disruptions in case it is chosen but turns off unexpectedly, stalling the process. The node begins by syncing cycle data from the network, followed by syncing account data relevant to it from other nodes. After completing this process, the node notifies the network that it has finished syncing. If a node fails to complete syncing within a specified threshold (typically double the median sync time of active nodes), it is subject to slashing and removal from the network. The syncing process can be examined in [`sync()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Sync.ts#L97).
* **Activation Process:** Once syncing is complete, nodes do not immediately become active due to the complexities involved in sharding calculations. These calculations could significantly disrupt the address space, creating conflicts over which nodes possess the necessary data. To maintain a steady rate of activation, only a certain number of nodes are allowed to go active in each cycle. This helps to smooth out potential waves and inconsistencies. The data synced from the queued nodes is discarded, as the likelihood of a node being assigned to the same shard in the next cycle is very low. Additionally, the time it might take for this node to be randomly selected again could be substantial. The activation process can be seen in [`updateRecord()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Active.ts#L146).



### 5. Handling Node Failures and Loss Detection

The lost node detection system kicks into action when a node (Node A) attempts to communicate with another node (Node B) and receives no response. This could indicate that Node B is down, experiencing connectivity issues, or it is malicious.

#### 5.1 Initial Detection

* **Communication Attempt**: When Node A sends a message to Node B and gets no response, it tentatively classifies Node B as potentially lost. The node is reported as ‘lost’ via the [`reportLost()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Lost.ts#L705C10-L705C20) function.
* **Waiting Period**: Node A waits a specified amount of time, typically until the next cycle, to allow for temporary issues at Node B to resolve themselves.

#### 5.2 Verification Process

* **Deterministic Selection of Verifiers**: To verify the status of Node B, Node A requests assistance from four other nodes. These nodes are selected based on deterministic algorithms linked to the network's sharding calculations. The checkers nodes are picked in [`getMultipleCheckerNodes()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Lost.ts#L782).
* **Stealth Checks**: The selected nodes (verifiers) perform stealth checks on Node B. These aren’t plain “Are you down?” checks, but rather requests for some data, made so that the verification requests won't be distinguishable from other requests the node receives, ensuring a fair response.

#### 5.3 Reporting and Consensus

* **Response Analysis**: If Node B is indeed down or unresponsive, it will fail to respond to the stealth checks. The verifiers report their findings back to Node A. The report is checked in the [`checkReport()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Lost.ts#L1008).
* **Partial Consensus**: Unlike traditional Byzantine fault tolerance mechanisms that require a full consensus, this system only needs a simple majority (e.g., two out of four verifiers) to classify Node B as lost. The majority of the logic about marking a node officially as lost is handled by [`lostReportHandler()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Lost.ts#L861).

#### 5.4 Record and Refutation

* **Updating Network Records**: If Node B is confirmed as lost, this information is included in the cycle record as a 'lost' node. This record informs all network participants of the node’s status.
* **Opportunity for Refutation**: Node B has the opportunity to refute its 'lost' status in the next cycle. If it can demonstrate it is operational (e.g., by responding to network requests), it will be removed from the lost list and reintegrated into the active list.
* **Finality of Status**: If Node B fails to refute its status, it is permanently removed from the active node list, ensuring that only functional and honest nodes participate in network activities.
* The status of the node is updated in [`updateRecord()`](https://github.com/shardeum/shardus-core/blob/8e69984a098c8ed387ca8f0684b313a75f85aec4/src/p2p/Lost.ts#L429).

![Cycle](/img/new/i31.jpg)

#### 5.5 Prevention of Abuse

* **Slashing for False Positives**: To discourage nodes from repeatedly going offline and coming back, the network implements a slashing mechanism. Nodes that frequently fail and then refute their lost status may face penalties.
* **Complexity of Deception**: The system is designed so that maintaining a facade of activity (for nodes attempting to appear active without performing their duties) is as labor-intensive as actually participating in the network. This discourages fraud and promotes genuine participation.
