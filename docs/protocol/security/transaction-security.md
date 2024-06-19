---
title: Transaction Security
sidebar_position: 4
image: /img/new/i48.png
---

# Transaction Security

### 1. Overview

Transaction security is paramount in Shardeum to ensure the validity, integrity, and authenticity of each transaction. This section outlines the various measures implemented in Shardeum to guarantee secure and valid transactions, including mechanisms like cryptographic hashing, timestamping, and nonce handling.

### **2. Transaction Lifecycle and Nonce Handling**

Shardeum's transaction lifecycle includes several security features to prevent unauthorized transactions and replay attacks:

* **Nonce Mechanism:** Each transaction includes a nonce, which is a sequential number associated with the sender’s account. This mechanism ensures that each transaction is unique and prevents replay attacks, where an attacker might try to resubmit a transaction multiple times. Transactions with nonces that are out of order are held until the preceding transactions are processed, maintaining the correct sequence.
* **Signature Verification:** Transactions must be signed with the sender’s private key. This cryptographic signature ensures that only the account holder can initiate transactions from their account, preventing unauthorized transactions. The signature is verified by validators before the transaction is processed, ensuring authenticity.

![Nonce](/img/new/i48.png)

### **3. Cryptographic Hashing**

Cryptographic hashing is a fundamental component of transaction security in Shardeum. It ensures data integrity and non-repudiation by producing a fixed-size hash value from the transaction data:

* **Hash Generation:** When a transaction is created, a cryptographic hash of the transaction data is generated. This hash acts as a unique identifier for the transaction and is used to ensure that the data has not been altered. Even a small change in the transaction data will result in a completely different hash, making tampering easily detectable.
* **Hash Verification:** Validators use the transaction hash to verify the integrity of the transaction data during the validation process. The hash ensures that the transaction data received is exactly as the sender intended, with no alterations.

### **4. Timestamping**

Timestamping is another crucial security measure in Shardeum, ensuring the chronological order of transactions and preventing double spending:

* **Transaction Timestamp:** Each transaction is assigned a timestamp when it is submitted to the network. This timestamp records the exact time the transaction was created, helping to maintain the chronological order of transactions.
* **Order Verification:** Validators check the timestamps of transactions to ensure they are processed in the correct order. This prevents issues like double spending, where a user might attempt to spend the same funds multiple times. By verifying timestamps, validators can ensure that only the first transaction is valid and subsequent attempts to spend the same funds are rejected.

### **5. Access Lists and Memory Management**

Shardeum uses access lists to manage and control memory access during transaction execution. This ensures that transactions do not interfere with each other, preventing unauthorized data manipulation:

* **Predictive Access Lists:** Before execution, transactions provide access lists indicating which parts of memory they will touch. This helps schedule transactions and avoid conflicts by ensuring that no two transactions access the same memory at the same time.
* **Runtime Verification:** During execution, if a transaction tries to access memory not listed in its access list, it is flagged and potentially rejected. This prevents unauthorized data access and manipulation, ensuring that transactions only affect the data they are supposed to.

![Access List](/img/new/i49.png)

### **6. Dynamic State Sharding and Data Healing**

Shardeum's dynamic state sharding ensures balanced resource usage and prevents bottlenecks. Data integrity across shards is maintained through:

* **Data Healing:** Nodes periodically compare their state against hashes of data held by other nodes. If discrepancies are detected, nodes request the correct data from their peers and update their state accordingly. This ensures that all nodes maintain a consistent and accurate view of the blockchain state.
* **Efficient Hashing:** Shardeum employs a custom hashing algorithm suitable for a sharded network, allowing efficient verification and repair of data across shards. This hashing ensures that data integrity is maintained even as transactions are distributed and processed across multiple shards.

![State Healing](/img/new/i50.png)

### **7. Networking and Gossip Protocol**

Shardeum uses a custom networking library and a gossip protocol to ensure efficient and secure communication between nodes:

* **Shardeum Net:** This internal protocol handles socket connections between nodes, with optimizations for performance and security. It ensures that messages and transaction data are securely transmitted across the network.
* **Gossip Protocol:** For broadcasting messages efficiently, Shardeum uses a gossip protocol where messages propagate quickly and redundantly across the network. This reduces the chance of missing critical information and ensures that all nodes receive transaction data promptly.

![Gossip](/img/new/i51.png)

### **8. Transaction Validation**

Each transaction goes through strict validation rules before being confirmed on the network. This includes checks for transaction integrity, authenticity, and compliance with network rules.

![Validation](/img/new/i54.png)

### **9. Consensus on Transaction Validity**

Shardeum's architecture requires that all nodes participating in the consensus process agree on the validity of transactions, ensuring no single node can approve incorrect transactions.

Shardeum uses a hybrid consensus mechanism combining Proof of Stake (PoS) and Proof of Quorum (PoQ). This approach ensures that transactions are validated by a quorum of nodes that must collectively agree on the transactions. Nodes participate in the consensus process by staking SHM, the native token, and can be penalized by losing their stake if they act maliciously. This staking requirement helps prevent Sybil attacks, where an attacker can flood the network with nodes under their control.

Although Shardeum processes transactions individually, it still generates blocks at certain intervals to support compatibility with smart contracts. These blocks use timestamps to map transactions deterministically, adding an additional layer of temporal security to transaction processing.

![Transaction Validity](/img/new/i53.png)
