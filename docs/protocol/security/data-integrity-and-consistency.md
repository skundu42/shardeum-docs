---
title: Data Integrity and Consistency
sidebar_position: 2
image: /img/new/i55.jpg
---

# Data Integrity and Consistency

### 1. Overview

Data integrity and consistency are crucial for maintaining a secure and reliable blockchain network. In Shardeum, various mechanisms ensure that data remains accurate, consistent, and tamper-proof across all nodes. This section outlines how Shardeum maintains data consistency and integrity, with a focus on cryptographic methods.

### **2. Dynamic State Sharding and Data Healing**

Shardeum's dynamic state sharding is designed to balance resource usage and prevent bottlenecks while maintaining data consistency across all shards. The network dynamically assigns transactions to shards based on load, ensuring efficient processing. Data integrity across shards is maintained through:

* **Data Healing:** Nodes periodically compare their state against hashes of data held by other nodes. If discrepancies are detected, nodes request the correct data from their peers and update their state accordingly. This periodic comparison and correction process ensures that all nodes maintain a consistent and accurate view of the blockchain state.
* **Efficient Hashing:** Shardeum employs a custom hashing algorithm suitable for a sharded network, allowing efficient verification and repair of data across shards. This hashing ensures that data integrity is maintained even as transactions are distributed and processed across multiple shards.

![Data Healing](/img/new/i55.png)

### **3. Cryptographic Hashing**

Cryptographic hashing plays a fundamental role in ensuring data integrity in Shardeum. It ensures data remains unchanged and verifiable:

* **Hash Generation:** Each piece of data and each transaction generates a cryptographic hash. This hash acts as a unique identifier for the data, ensuring its integrity. Even a minor alteration in the data results in a completely different hash, making tampering easily detectable.
* **Hash Verification:** During the validation process, validators use the cryptographic hash to verify the integrity of the data. The hash ensures that the data received by validators is exactly as intended, with no alterations or tampering.

### **4. Timestamping**

Timestamping is another essential measure for maintaining data integrity and consistency in Shardeum:

* **Transaction Timestamp:** Each transaction is assigned a timestamp upon submission to the network. This timestamp records the exact time the transaction was created, ensuring the chronological order of transactions.
* **Order Verification:** Validators check the timestamps of transactions to ensure they are processed in the correct order. This prevents issues like double spending, where a user might attempt to spend the same funds multiple times. By verifying timestamps, validators ensure that only the first transaction is valid and subsequent attempts to spend the same funds are rejected.

### **5. Consensus on State**

To maintain data consistency, Shardeum employs a robust consensus mechanism that requires agreement among participating nodes:

* **Proof of Stake (PoS) and Byzantine Fault Tolerance (BFT):** Shardeum uses a hybrid consensus mechanism combining PoS and BFT. Validators are selected based on their stake and must collectively agree on the validity of transactions and state changes. This approach ensures that data consistency is maintained, as all changes to the blockchain state are agreed upon by a quorum of nodes.
* **Consensus on State Hashes:** Nodes share state hashes with each other periodically. These hashes represent the state of the blockchain data. By comparing these hashes, nodes can detect discrepancies and initiate data healing processes to correct any inconsistencies.

### **6. Access Lists and Memory Management**

Shardeum uses access lists to manage and control memory access during transaction execution, ensuring data consistency and preventing unauthorized data manipulation:

* **Predictive Access Lists:** Transactions provide access lists indicating which parts of memory they will touch before execution. This helps schedule transactions and avoid conflicts by ensuring that no two transactions access the same memory simultaneously.
* **Runtime Verification:** During execution, if a transaction tries to access memory not listed in its access list, it is flagged and potentially rejected. This prevents unauthorized data access and manipulation, ensuring that transactions only affect the data they are supposed to.

![Access Lists and Memory](/img/new/i56.png)

### **7. Cycle Records and Hashing**

Each cycle in the network records data changes, with a cryptographic hash taken over this data. These hashes link to previous cycle records, creating a secure and immutable record of all changes across the network.

### **8. Deterministic Algorithms**

The deterministic algorithms guarantee every node processes transactions and maintains data in a consistent manner.

#### **Data Synchronization and Validation**

Nodes constantly synchronize and validate their data against other nodes. This way, whenever discrepancies occur, they will be identified and corrected promptly. This ongoing process helps maintain the overall consistency of the network data.

![Access Lists and Memory](/img/new/i57.png)

#### **Networking and Gossip Protocol**

Shardeum uses a custom networking library and a gossip protocol to ensure efficient and secure communication between nodes, which is crucial for maintaining data consistency:

* **Shardeum Net:** This internal protocol handles socket connections between nodes, optimizing performance and security. It ensures that messages and transaction data are securely transmitted across the network.
* **Gossip Protocol:** For broadcasting messages efficiently, Shardeum uses a gossip protocol where messages propagate quickly and redundantly across the network. This reduces the chance of missing critical information and ensures that all nodes receive transaction data promptly.

![Networking & Gossip](/img/new/i58.png)
