---
title: Cycylic Operations
sidebar_position: 1
image: /img/new/evmframe1.jpg
---


# Cyclic Operations

### 1. Overview

Cyclic operations are a crucial component of the Shardeum blockchain network, enabling efficient and synchronized updates across all nodes. This model ensures that all nodes maintain a consistent view of the network state and can agree upon changes in a structured manner. The cycle-based operational model divides time into regular intervals called cycles, each consisting of four phases, or quarters. These phases help manage updates, synchronization, and validation processes systematically.

### 2. Node List and Cycle Chain

In Shardeum, maintaining an up-to-date and consistent node list is fundamental. Each node keeps a list of every other node, known as the **node list**, and a history of all changes made to this list, referred to as the **cycle chain**. Nodes suggest changes to the node list through signed messages called **updates**. These updates are collected and processed within the cycles.

The cycle chain records the progression and history of all cycles from the network's inception. Nodes must synchronize with the node list and cycle chain to participate in the network, suggesting updates and processing cycles collaboratively.

### 3. Cycle Creator Process Summary

The [`cycleCreator()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/CycleCreator.ts#L342) function is essential for managing the cyclic operations within Shardeum. Here's a concise explanation of the process:

1. **Initialization:** The function begins by initializing the current quarter and logging the current state.
2. **Fetching Previous Record:** It attempts to retrieve the previous cycle record. If unsuccessful, it retries until the record is fetched.
3. **Applying Previous Changes:** The function applies changes from the previous cycle record to the node list.
4. **Saving to Database:** It creates a cycle marker and certificate, saves the data to the database, and emits an event indicating new cycle data.
5. **Logging and Pruning:** The function logs the current cycle and node list state, then prunes the cycle chain to maintain an optimal size.
6. **Data Distribution:** It sends the latest cycle data to any subscribed archivers.
7. **Updating Cycle and Quarter:** The function updates the current cycle and quarter based on the previous record.
8. **Scheduling:** It schedules the execution of each quarter and the next cycle using calculated times, ensuring all nodes are synchronized.
9. **Final Logging:** The function concludes by logging the end of the cycle creation process.

This process ensures the network remains consistent and synchronized, handling transitions between cycles efficiently.

### 4. Phases of the Cycle

* **Quarter 1: Update Phase Start**
  * **Function:** [`runQ1()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/CycleCreator.ts#L496)
  * **Detailed Operation:**
    * Nodes use the `runQ1()` function to start collecting updates. This phase allows nodes to gather all the proposed changes, ensuring they have a comprehensive view of the network's current state.
    * Nodes collect these updates for a specified duration, ensuring that all potential changes are captured for the current cycle.
    * [`sendRequests()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/Lost.ts#L582) in `p2p/Lost.ts` is invoked:
      * **Lost Node Reporting:** It checks and reports nodes that are deemed lost based on certain conditions, using aggregated data collected until the first quarter (Q1).
      * **Scheduled Removals:** It handles and processes scheduled node removals, ensuring nodes are removed as necessary and deletes them from the tracking list.
      * **Gossiping Status Updates:** It gossips information about nodes that have been verified as down to other nodes in the network, and manages self-refutation messages to update the network about nodes that are back online.
    * [`sendRequests()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/Apoptosis.ts#L296) from `p2p/Apoptosis.ts` is invoked:
      * When a node decides to exit the network, it sends an Apoptosis message to around three other active nodes to notify them of its departure, allowing for immediate removal from their node lists. This message, which can be sent at any time, is verified and stored to be gossiped in the next quarter 1, ensuring the exiting node is quickly removed from the network's node list without waiting for the usual discovery process.
    * [`sendRequests()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/Active.ts#L262) from `p2p/Active.ts` is invoked:
      * This function handles the process of sending queued requests for a node to become active in the network. If a request is queued and the node is not restricted from becoming active, it signs the request and attempts to add it to the list of active transactions. It then gossips the signed request to other nodes. If the node does not become active within the expected cycle duration, it retries the request. If the node goes active, it stops the retry process.
    * [`sendRequests()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/LostArchivers/index.ts#L199) from `p2p/LostArchivers/index.ts` is invoked:
      * This function manages the `lostArchiversMap`. It logs the current state of the map for debugging, then iterates through each entry to handle different statuses. For 'reported' entries, it creates and sends an investigation message and removes the entry. For 'down' statuses that haven't been gossiped, it creates and gossips a down message, marking it as gossiped. Similarly, for 'up' statuses, it creates and gossips an up message, marking it as gossiped. This ensures accurate and timely updates about the status of archivers in the network.
    * Config queue changes and debug logic updates are being handled in a [listener](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/shardus/index.ts#L934-L947) inside the `shardus.ts` [`start()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/shardus/index.ts#L435) function.
    * Changes in network coverage are calculated and summaries of previous cycles are processed are being executed by [listener](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/state-manager/index.ts#L3703-L3740) in `state-manager.ts` `startShardCalculations()`.
* **Quarter 2: Update Phase End**
  * **Function:** [`runQ2()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/CycleCreator.ts#L526)
  * **Detailed Operation:**
    * The `runQ2()` function marks the end of this collection period, consolidating the updates for synchronization.
    * Node selection is triggered by [`executeNodeSelection()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/Join/v2/select.ts#L28-L38).
* **Quarter 3: Cycle Sync Start**
  * **Function:** [`runQ3()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/CycleCreator.ts#L561)
  * **Detailed Operation:**
    * During `runQ3()`, nodes compare their collected updates by exchanging hashes (`cycle_tx_hash`). This step ensures all nodes have a consistent view of the updates.
    * Nodes validate these updates by checking the `cycle_tx_hash` and creating a cycle certificate based on the highest value votes from other nodes. This certificate ensures that only valid updates are applied.
    * Nodes gossip their certificates to ensure the network agrees on the cycle's contents.
    * Cycle data is being [cleaned up](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/state-manager/index.ts#L3742-L3770) on every 5 cycles.
* **Quarter 4: Cycle Finalization**
  * **Function:** [`runQ4()`](https://github.com/shardeum/shardus-core/blob/e8c14ce4ba19785145646e840082acb57ec8ce3b/src/p2p/CycleCreator.ts#L634)
  * **Detailed Operation:**
    * The `runQ4()` function handles the final certification of the updates. Nodes certify the agreed updates, creating a cycle marker and certificate to log the cycle's changes.
    * This phase ensures the network's integrity by preventing double voting and other malicious activities through a robust consensus mechanism.
    * Nodes prune the cycle chain to keep it within a manageable size, typically retaining a set number of recent cycles.

![Cycle](/img/new/i8.jpg)



