---
title: Node Security & Integrity
sidebar_position: 4
image: /img/new/i41.jpg
---

# Node Security and Integrity

### 1. Overview

Node security and integrity are critical components of the Shardeum blockchain, ensuring that nodes operate correctly and the network remains secure against unauthorized or malicious activities. The following sections detail the comprehensive security measures implemented in Shardeum to prevent unauthorized or malicious behavior by nodes.

### **2. Node Authentication and Authorization**

Every node in the Shardeum network must be authenticated before joining. The authentication process checks the legitimacy and integrity of the nodes to ensure compliance with the network's standards. This process guarantees that only legitimate and verified nodes can participate in network activities, reducing the risk of malicious entities infiltrating the network.

![Node Authentication](/img/new/i41.png)

### **3. Validator Security**

Shardeum employs a system where every active validator must know all other active validators. This control and visibility prevent unauthorized nodes from participating in the consensus process, ensuring only verified nodes can validate transactions.

![Validator Security](/img/new/i43.png)

### **4. Permissioned Standby List**

Nodes in the standby list are permissioned, meaning their credentials and roles are pre-verified before they can become active validators. This ensures that the standby nodes meet all network security requirements before activation, preventing infiltration by malicious nodes.

![Permissioned Standby List](/img/new/i44.png)

### **5. Deterministic Lottery for Node Selection**

The transition from standby to active nodes is governed by a deterministic lottery system. This system is designed to be tamper-proof, making it impossible for standby nodes to manipulate their selection. The lottery is based on cryptographic principles that ensure fairness and randomness, preventing any single node from gaining an unfair advantage.

### **6. Consensus Mechanism**

Shardeum employs a combination of Proof of Stake (PoS) and Byzantine Fault Tolerance (BFT) for its consensus mechanism. This hybrid approach ensures that nodes participating in the consensus process are both financially incentivized and cryptographically secured to act honestly. Validators are selected based on their stake, and BFT consensus ensures fast and secure transaction finality. This combination reduces the risk of Sybil attacks, where an attacker might create multiple fake nodes to gain control of the network.

### **7. Slashing Mechanisms**

Shardeum has implemented robust slashing mechanisms to penalize nodes that exhibit malicious behavior or fail to perform their duties correctly. Key slashing rules and their enforcement include:

* **Early Exit Slashing**: If a node leaves the network before completing its assigned tasks, it is subject to slashing. This ensures nodes remain active and fulfill their responsibilities.
* **Double Voting**: If a node sends conflicting votes for the same transaction (e.g., voting both for and against it), this indicates malicious behavior. Such actions are detected, and the node is slashed accordingly.
* **Lazy Node Detection**: Nodes that do not perform their required work but remain in the network to earn rewards are considered lazy. Shardeum employs various checks to detect such nodes. If a node is flagged as lazy and refutes the claim repeatedly without justification, it gets slashed incrementally until it is removed from the network.

These slashing rules ensure that nodes operate honestly and contribute positively to the network's security and integrity.

### **8. Lost Node Detection and Refutation**

To maintain network stability, Shardeum has a system to detect and handle lost nodes. A node is considered lost if it fails to respond to requests. The detection process involves:

* **Stealth Checks**: Four deterministic nodes perform stealth checks by sending requests to the suspected lost node. These requests mimic regular traffic to avoid detection by malicious nodes.
* **Verification and Consensus**: If a sufficient number of these checking nodes confirm the node's unresponsiveness, it is marked as lost. The lost node is then removed from the network unless it refutes the claim.

Refutation involves the node proving its activity and rejoining the network. Persistent failure to refute leads to penalties and eventual removal.

### **9. Transaction Lifecycle and Nonce Handling**

Shardeum's transaction lifecycle includes several security features to prevent unauthorized transactions and replay attacks:

* **Nonce Mechanism**: Each transaction includes a nonce, which is a sequential number to prevent replay attacks. Transactions with nonces out of order are held until the preceding transactions are processed.
* **Signature Verification**: Transactions must be signed with the sender’s private key, ensuring that only authorized parties can initiate transactions from an account.

### **10. Access Lists and Memory Management**

Shardeum uses access lists to manage and control memory access during transaction execution. This ensures that transactions do not interfere with each other, preventing unauthorized data manipulation:

* **Predictive Access Lists**: Before execution, transactions provide access lists indicating which parts of memory they will touch. This helps schedule transactions and avoid conflicts.
* **Runtime Verification**: During execution, if a transaction tries to access memory not listed in its access list, it is flagged and potentially rejected. This prevents unauthorized data access and manipulation.

### **11. Dynamic State Sharding and Data Healing**

Shardeum's dynamic state sharding ensures balanced resource usage and prevents bottlenecks. The network dynamically assigns transactions to shards based on load. Data integrity across shards is maintained through:

* **Data Healing**: Nodes periodically compare their state against hashes of data held by other nodes. If discrepancies are detected, nodes request the correct data from their peers and update their state accordingly.
* **Efficient Hashing**: Shardeum employs a custom hashing algorithm suitable for a sharded network, allowing efficient verification and repair of data across shards.

### **12. Node Lifecycle Management and Certificates**

The node lifecycle in Shardeum includes several steps to ensure only legitimate nodes participate in the network:

* **Certificate-Based Staking**: Nodes must obtain certificates to join the network. This involves staking tokens, getting certificates from existing nodes, and passing readiness checks.
* **Standby and Active Modes**: Nodes join the network in standby mode and undergo a rigorous syncing process before becoming active. This includes syncing cycle data and account data, ensuring they are up-to-date and ready to participate in consensus.

### **13. Networking and Gossip Protocol**

Shardeum uses a custom networking library and a gossip protocol to ensure efficient and secure communication between nodes:

* **Shardeum Net**: This internal protocol handles socket connections between nodes, with optimizations for performance and security.
* **Gossip Protocol**: For broadcasting messages efficiently, Shardeum uses a gossip protocol where messages propagate quickly and redundantly across the network, reducing the chance of missing critical information.

### **14. Periodic Patching and Consensus on State**

To maintain data consistency and integrity:

* **Periodic Patching**: Nodes engage in periodic state checks using a tree structure to identify and correct discrepancies.
* **Consensus on State**: Nodes share state hashes with each other and repair any mismatches through efficient data requests, ensuring all nodes have the correct and consistent state data.
