---
title: Validator Operations
sidebar_position: 1
image: /img/new/i9.jpg
---

# Validator Operations

### 1. Overview

Validators in the Shardeum network play a pivotal role in maintaining the blockchain's integrity and efficiency. By managing state storage, processing transactions, participating in consensus mechanisms, and upholding the rules through staking and slashing, validators ensure the network operates smoothly and securely.

![validator Operations](/img/new/i9.jpg)

### 2. State Storage by Validators

*   **Current State Focus:** Validators are responsible for storing the latest state of accounts within the Shardeum network. Unlike some networks that require storing the entire historical state, Shardeum validators focus on the current state.

    **Example:** Imagine a validator that is responsible for maintaining the state of 1,000 accounts. If Account A initially has 100 SHM and transfers 0.01 SHM to Account B, the validator updates Account A's balance to 99.99 SHM and Account B's balance to reflect the additional 0.01 SHM. The validator does not keep a history of the previous balances, only the current state.
* **Role of Archive Servers:** Historical data is managed by Archive Servers, which are designed to handle larger storage capacities and can manage vast amounts of historical state data without burdening the validators.
* **Detailed Data Storage:** Validators manage various types of data, including user account data and smart contract data. Each piece of data is assigned a 32-byte address and is stored as account data at the protocol level. This means that any data a transaction can generate or modify must be stored by validators.
* **Dynamic Sharding:** Validators manage state by utilizing a sharding mechanism where the account data is divided among various nodes. Each node is responsible for storing a portion of the total state, making the process efficient and scalable. This dynamic sharding ensures that the network can grow and handle more transactions without overloading individual nodes.

![Validator Operations](/img/new/i10.jpg)

#### 2.1 Account Storage

* **Basic Structure:** In the EVM, user accounts include externally owned accounts (EOAs) that hold balances and nonces. These accounts enable users to send transactions and manage their holdings.
*   **Merkle Patricia Trees:** The EVM uses Merkle Patricia Trees to organize and manage account storage. This structure enables efficient verification of the data integrity and state transitions by providing cryptographic proofs for account balances and nonces.

    **Example:** When a user sends a transaction, the state changes (like balance updates) are recorded in the Merkle Patricia Tree. This ensures that the entire state can be efficiently verified and the history of state changes can be traced back cryptographically.

#### 2.2 Contract Storage

* **Smart Contract Accounts:** Smart contracts have specific storage requirements. A smart contract account holds the balance and nonce, and includes a pointer to the code bytes. The code bytes contain the actual machine code of the smart contract.
* **Code Bytes:** The code for smart contracts is stored in a type of account called Code Bytes. This is essentially the compiled Solidity code in the form of EVM bytecode. Thus it’s the heaviest one.
* **Contract Storage:** Smart contracts also maintain storage, which holds key-value pairs. Each key and value is 256 bits in size, and this storage is used to maintain the state specific to the smart contract.
*   **Merkle Patricia Trees in Contract Storage:** Similar to account storage, contract storage uses Merkle Patricia Trees to organize data efficiently. This ensures that every storage operation can be traced and verified.

    **Example:** If a smart contract manages a token balance, each user's balance is stored as a key-value pair in the contract storage. The key is the user's address, and the value is their token balance. Changes to these balances are recorded in the Merkle Patricia Tree, allowing for efficient verification of the entire contract state.
* **Complex Data Structures:** For more complex data structures, such as arrays or long strings, the compiler breaks down the data into 256-bit segments. These segments are then stored across multiple contract storage accounts.

![Contract Storage](/img/new/i11.jpg)

#### 2.3 Global Account Storage

* **Purpose:** Validators also handle global accounts, which store critical network-wide settings. These accounts are accessible and verifiable by all nodes, ensuring consistent network parameters and rules.
* **Global Settings:** A global account might store the current protocol version, minimum staking requirements, or network upgrade schedules. This data is critical for maintaining network coherence and ensuring all validators operate under the same rules.

### 3. Transaction Processing

#### 3.1 User Transactions

* **Transaction Injection:** User transactions in Shardeum are primarily interactions with the EVM (Ethereum Virtual Machine) and include actions such as running smart contracts or transferring tokens. When a transaction is initiated through an RPC server, it is injected into the validator network. One active validator receives the transaction and distributes it to other validators within the transaction group.
* **Transaction Distribution:** Upon receiving the transaction, the active validator sends this transaction to other validators within the transaction group. These validators then process the transaction independently but follow the same deterministic rules, ensuring consistency in the outcome.
* **Transaction Execution:** Validators apply the transaction by executing the EVM bytecode associated with the transaction. This involves loading the necessary account data, performing the operations specified by the transaction, and updating the account states accordingly.
* **Consensus and Receipt Generation:** After processing the transaction, validators create a vote indicating whether the transaction was successful or if it should be rejected. These votes are then shared among the validators to form a consensus.
* **Data Distribution:** Once a transaction has been processed and a receipt generated, this information is forwarded to the archive servers. Archive servers store this data and distribute it to other network components, such as data explorers and RPC servers, ensuring that the transaction status is updated across the entire network.
* **Transaction Group Formation:** In Shardeum, the formation of transaction groups is a dynamic process where validators from different consensus groups come together to process a transaction. This ensures that each transaction is handled by a subset of validators responsible for the relevant account data.
* **Error Handling and Resilience:** The Shardeum network is designed to handle errors and ensure resilience in transaction processing. Validators have mechanisms to detect failed transactions, retry processing, and ensure that the network remains stable even under adverse conditions.

#### 3.2 Internal Transactions

* **Network Maintenance Transactions:** Internal transactions, essential for the network's maintenance and reward systems, are generated by the validators themselves. These include transactions for staking, unstaking, and tallying rewards.

![Internal Transaction](/img/new/i12.jpg)

### 4. Consensus Mechanisms

*   **Consensus Groups:** Shardeum employs a unique consensus mechanism involving consensus groups and transaction groups. A consensus group is a subset of validators responsible for a specific range of account data. This sharding approach allows the network to handle a higher transaction throughput by dividing the workload among multiple validators.

    **Example:** In a Shardeum network with 1,024 nodes and a consensus group size of 128, each consensus group handles a portion of the total account data. For instance, if a consensus group is responsible for accounts with addresses ranging from 0x00 to 0x0F, these 128 validators work together to process transactions involving these accounts.
* **Deterministic Algorithms:** The consensus mechanism in Shardeum is designed to ensure that all validators work together efficiently. This involves using deterministic algorithms to select validators for each transaction group, ensuring that the network can scale dynamically and handle varying loads.
* **Dynamic Sharding:** Shardeum uses dynamic sharding to ensure that the network can scale and manage loads effectively. This approach allows validators to join or leave the network, and the consensus mechanism adjusts accordingly to maintain efficiency and security.

### 5. Staking and Rewards

* **Staking Process:** Staking in Shardeum involves users locking up their tokens to become validators and participate in the network's operations. The staking process is facilitated through transactions signed via a private key.
* **Rewards Calculation:** Validators earn rewards based on their participation and performance in the network. Rewards are calculated and tallied periodically. When a validator node goes active, completes its cycle, and eventually rotates out, the network calculates the rewards earned during its active period.
* **Claiming Rewards:** To claim rewards, users must initiate an unstaking transaction, which transfers the rewards to their account balance after verifying that all conditions are met (e.g., the node is not currently active).

![Staking](/img/new/v3551.jpg)

### 6. Slashing Mechanisms

* **Purpose of Slashing:** Slashing serves as a penalty for validators that exhibit malicious or undesirable behavior. This includes actions such as leaving the network prematurely or attempting to double vote on transactions. When such behavior is detected, the network imposes a penalty, reducing the validator's stake.
* **Examples of Slashing Conditions:**
  * **Early Exit:** Validators that leave the network before completing their assigned duty cycle are penalized to discourage instability.
  * **Double Voting:** Validators attempting to double vote on a transaction are penalized, as this behavior can undermine the consensus process.
  * **Lazy Nodes:** Validators that fail to participate actively or meet their responsibilities can be penalized to ensure network efficiency and reliability.
* **Slashing Process:** Slashing involves submitting a transaction to the network, detailing the offense and the proposed penalty. Other validators then vote on this transaction to form a consensus. If agreed upon, the penalty is enforced, and the validator's stake is reduced accordingly. This process ensures that all changes to the state are recorded through consensus, maintaining the integrity of the distributed ledger.

![Slashing Mechanisms](/img/new/validator1.jpg)