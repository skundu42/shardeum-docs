---
title: Security in a Sharded Network
sidebar_position: 1
image: /img/new/i59.jpg
---

# Security in a Sharded Network

### 1. D**ata Healing via Sharded Hash Tree (Advanced Security Mechanism)**

Shardeum uses a unique sharded hash tree (TRIE) to maintain data integrity. Nodes regularly exchange intermediate hashes to check for data consistency. When discrepancies are detected, nodes efficiently search the tree to locate and correct specific errors.

If nodes in a transaction group find inconsistencies in transaction receipts or states, they can trigger a repair process. This involves requesting and validating the correct data from other nodes, ensuring the network stays synchronized and reliable.

![Sharded Hashed Tree](/img/new/i59.png)

Given that Shardeum functions as a sharded network, processing various transaction sets across its multiple shards, its security model diverges from non-sharded Layer 1 architectures. It must defend against both standard Layer 1 threats and those specific to sharded architectures. Below we will investigate the common attacks and how Shardeum mitigate them.

### **2. Sybil Attacks**

* **Description:** Sybil attacks happen when attackers create numerous fake identities to gain control of the network, disrupting its security.

![Sybil](/img/new/i60.png)

* **Mitigations:** Shardeum tackles Sybil attacks by requiring each node to stake a certain amount of SHM tokens. This economic requirement makes it costly for attackers to create many fake nodes. Additionally, nodes that act maliciously are penalized and removed from the network, increasing the difficulty and cost of repeated attacks.

### **3. Shard Takeover Attack**

* **Description:** This attack involves an adversary filling a shard with their own nodes to gain control. With 33% control, they can halt the shard; with 66%, they can forge transactions.
* **Mitigations:** Shardeum prevents this by randomly selecting and rotating nodes. Nodes can't choose their shard, and achieving 66% control would mean taking over the entire network, which is economically unfeasible due to staking requirements.

### **4. Nothing at Stake**

* **Description:** In a network fork, validators might validate on both chains without incurring costs, unlike in PoW networks where resources prevent such behavior.
* **Mitigations:** Shardeum uses Proof of Quorum (PoQ) for consensus and Proof of Stake (PoS) as a deterrence against Sybil attacks. The network doesn’t follow the longest-chain rule and penalizes double-signing, preventing such forks.

### **5. Long Range Attacks**

* **Description:** Attackers create a fork from the genesis block to build a competing chain.
* **Mitigations:** Shardeum’s PoQ and PoS mechanisms prevent such forks, ensuring the network only follows the legitimate chain. Digital signatures ensure transaction integrity, preventing long-range attacks.

### **6. Censorship**

* **Description:** A validator may prevent certain transactions from being processed.
* **Mitigations:** Shardeum’s architecture avoids this by not having blocks or leaders, meaning no single validator can control transaction inclusion. Effective censorship would require 33% control of a shard, which is countered by staking, node rotation, and random shard assignment.

### **7. DoS or DDoS Attack**

* **Description:** These attacks knock nodes offline, disrupting network activity.
* **Mitigations:** Nodes should use ISPs with robust DDoS protection. Shardeum’s design includes redundancy, allowing other nodes to validate transactions if some are down. New nodes can join the network each cycle, mitigating long-term impacts.

### **8. Transaction Flooding**

* **Description:** Attackers flood the network with valid transactions to slow it down.
* **Mitigations:** Shardeum imposes SHM gas fees that, while affordable for normal use, make it expensive to flood the network. Additionally, excessively active accounts may face higher fees, deterring attackers without affecting regular users.
