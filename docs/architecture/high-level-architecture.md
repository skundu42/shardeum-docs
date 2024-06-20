---
title: High-level Architecture
sidebar_position: 6
image: /img/new/coreframe1.png
---



# High-Level Architecture

![Shardeum Core Frame](/img/new/coreframe1.png)

### **1. What the Core Protocol does**:

* **Handling sharding and consensus:** Shardus Core Protocol focuses on managing sharding and making sure that all parts of the network agree (consensus). It's designed to not worry about the specific details of what's happening in transactions on the dapps that run on Shardeum.
* **Staying neutral:** The protocol doesn’t deal directly with the data or actions of transactions in dapps. It uses a set of simple commands (APIs) that help it stay out of the specifics, meaning it doesn’t know or need to know what each transaction is about.

### 2. **DApps and their interaction with Shardus Core Protocol**

* **Connecting through APIs:** Dapps on Shardeum, especially those that work like Ethereum Dapps, talk to the core protocol using a specific set of commands known as an API. This lets the Dapps use important features like agreement and data accuracy, while they can still do their own thing for their specific needs. [Here you can see all of them.](../protocol/apis-and-interfaces.md)
* **DApps keep their own rules:** Each dapp on Shardeum has its own set of rules and logic that fit what the dapp is designed to do. This setup means that even if dapps change or update, they don’t mess with the core protocol. This makes it easier to add new features to dapps without complicating things.

### 3. **Benefits of Shardeum’s design**:

* **Freedom to create:** Developers can build new and varied dapps without being limited by the system underneath. This freedom means they can quickly adapt to new needs and ideas.
* **Easy to grow:** Because the core protocol stays out of the dapp details, Shardeum can grow and improve without being bogged down by changes in individual dapps.
* **Safe and stable:** Keeping the core system separate from the dapps means the whole network is safer and more stable. It protects the core from any risky or poorly made dapp code.

### **4. More about how it works**:

* **Handling data:** The core protocol takes care of the big picture stuff like making sure all parts of the network are working together. It doesn’t store or deal with specific dapp data; dapps handle their own data and only talk to the core protocol for making sure everyone agrees.
* **Managing nodes:** The rules about how nodes (the individual parts of the network) operate are detailed in the protocol's documentation. [The core protocol manages whether nodes are active or on standby](../protocol/node-lifecycle.md), but it doesn’t get involved in the specific data that nodes deal with for dapps.
