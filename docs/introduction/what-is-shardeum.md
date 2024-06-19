---
title: Overview
sidebar_position: 1
image: /img/new/evmframe1.jpg


---

# What is Shardeum?

Shardeum is an EVM based L1 that uses dynamic state sharding to achieve linearly scalability while attaining atomic composability across shards. This means Shardeum can increase its TPS capacity with each validator added to the network to retain low fees forever.

Shardeum provides the highest throughput capacity of any EVM based L1 without sacrificing on decentralization. Developers can deploy and interact with Solidity or Vyper contracts without special considerations for sharding, since contracts are deployed to unique shards automatically while retaining atomic composability across all shards.

Shardeum is the solution to the blockchain trilemma. As blockchain tries to achieve scalability, decentralization and security, it will only be able to attain two. There must be a trade-off between decentralization and scalability with security an essential element. Shardeum achieves scalability, decentralization and still maintains security.

Shardeum’s auto-scaling feature allows the network to automatically adjust the number and size of shards based on the current workload. This allows the system to optimize performance and maintain high levels of scalability as it grows and evolves.

### 1. Integration

* Shardeum is built on the foundation of the Shardus Core Protocol, presenting itself as the first [EVM](https://ethereum.org/en/developers/docs/evm/)-compliant Layer 1 blockchain, optimized for dynamic sharding.
* This architecture uniquely enables the network to scale transaction throughput linearly with the addition of nodes, addressing the scalability issues commonly faced by traditional blockchains.
* Shardeum is designed to support a diverse array of DApps, making it a versatile platform for developers and businesses.

### 2. Shardeum's Features

* **Dynamic State Sharding:** This is the most advanced and complex way to shard the state of a network because it shards the State, Network, and Transactions and that too dynamically, as opposed to, in a pre-determined way.\

* **Linear Scalability:** Means a subset of the broader concept of scalability. Specifically, linear scalability refers to the fact that a system can increase its throughput as you provide it with more resources, and the relationship between throughput and resources is linear.\

* **Low Transaction Fees:** The minimal costs associated with processing and confirming transactions on a blockchain network. These fees are paid by users to incentivize miners or validators to include their transactions in the next block of the blockchain.\

* **EVM-based Smart Contract Platform:** Refers to the compatibility of a blockchain network with the Ethereum Virtual Machine (EVM), which allows it to execute smart contracts and transactions that are compatible with Ethereum's existing ecosystem.\

* **Solves Scalability Trilemma:** Solving the scalability trilemma in blockchain involves achieving a balance between decentralization, security, and scalability. Typically, enhancing scalability can compromise either decentralization or security, but innovative solutions like sharding, layer 2 protocols, and consensus algorithm improvements aim to address all three aspects simultaneously.\

* **Auto Scaling:** Adjusts computational resources automatically based on demand, ensuring optimal performance and cost efficiency. It handles traffic spikes efficiently, providing a seamless user experience without manual intervention.\

* **Security:** Security refers to the protection of data integrity, transaction authenticity, and network resilience against attacks. It ensures that transactions are tamper-proof and that the network is resistant to hacks, double-spending, and other malicious activities. Strong cryptographic protocols and consensus mechanisms are fundamental to maintaining high security in blockchain systems.\

* **Immediate Finality:** This means that once a transaction is confirmed, it cannot be altered, reversed, or double-spent. This ensures that transactions are permanently recorded and trusted as soon as they are included in a block. Immediate finality is crucial for applications requiring quick and definite transaction settlements.\

* **Low Latency:** This refers to the minimal delay between the initiation of a transaction and its confirmation. It is essential for applications needing quick transaction processing and user responsiveness. Achieving low latency helps improve the user experience and the overall efficiency of the blockchain network.\

* **Low Bandwidth:** The network can operate efficiently with minimal data transmission requirements. This is important for reducing operational costs and enabling access in regions with limited internet infrastructure. Optimized data protocols and lightweight transactions help achieve low bandwidth usage.\

* **High Fairness:** This ensures that all participants have equal opportunities to validate transactions and propose new blocks. It prevents any single entity from gaining disproportionate control over the network. Fairness is maintained through decentralized consensus mechanisms and transparent protocols.\

* **High Capacity:** This refers to the network's ability to handle a large volume of transactions per second (TPS). This is crucial for scalability, enabling the network to support a growing number of users and applications without performance degradation. Enhancements like sharding and layer 2 solutions help increase blockchain capacity.

### **3. Shardus Core Protocol**
* **Base protocol:** [The Shardus Core Protocol](https://shardus.com/) handles sharding and consensus without needing to know the specifics of application-level transactions. This separation ensures modularity and security, allowing the protocol to function independently of the DApps running on top of it. \
  Here, you can [take a look](https://github.com/shardeum/shardeum/blob/cd230c50859d28d17b3d013fee52923ba6c495aa/src/index.ts#L3354-L7347) at how we successfully integrate the Shardus software.

```
/***
 *     ######  ##     ##    ###    ########  ########  ##     ##  ######  
 *    ##    ## ##     ##   ## ##   ##     ## ##     ## ##     ## ##    ## 
 *    ##       ##     ##  ##   ##  ##     ## ##     ## ##     ## ##       
 *     ######  ######### ##     ## ########  ##     ## ##     ##  ######  
 *          ## ##     ## ######### ##   ##   ##     ## ##     ##       ##  
 *    ##    ## ##     ## ##     ## ##    ##  ##     ## ##     ## ##    ## 
 *     ######  ##     ## ##     ## ##     ## ########   #######   ######   
 */
const shardusSetup = (): void => {
  /**
   * interface tx {
   *   type: string
   *   from: string,
   *   to: string,
   *   amount: number,
   *   timestamp: number
   * }
   */
  shardus.setup({
    sync: sync(shardus, evmCommon),
    validateTransaction: validateTransaction(shardus),
    validateTxnFields: validateTxnFields(shardus, debugAppdata),
    isInternalTx,
    async apply(timestampedTx: ShardusTypes.OpaqueTransaction, wrappedStates, originalAppData) {
```


* **API interaction:** DApps interact with the Shardus Core Protocol through a [defined API](../protocol/apis-and-interfaces.md). This abstraction ensures that the core protocol does not directly access or manipulate application data, maintaining a clear boundary between the protocol and the applications.

![EVM Frame](/img/new/evmframe1.jpg)

### **4. EVM Compliance**

![EVM Frame](/img/new/evmframe2.jpg)

* **Smart contracts:** Shardeum supports the execution of [Ethereum](https://ethereum.org/en/developers/docs/) smart contracts, allowing developers to deploy their existing contracts without modification.&#x20;
* **Tool compatibility:** All tools and applications built for Ethereum can run on Shardeum, facilitating easy migration and adoption.
* **Execution environment:** By maintaining EVM compatibility, Shardeum leverages the robustness and familiarity of the Ethereum ecosystem, making it accessible for developers.
* **EVM Initialization:** [Click here to examine the code in our repos.](https://github.com/shardeum/shardeum/blob/cd230c50859d28d17b3d013fee52923ba6c495aa/src/index.ts#L405-L512)

```typescript
/***
 *    ######## ##     ## ##     ##    #### ##    ## #### ########
 *    ##       ##     ## ###   ###     ##  ###   ##  ##     ##
 *    ##       ##     ## #### ####     ##  ####  ##  ##     ##
 *    ######   ##     ## ## ### ##     ##  ## ## ##  ##     ##
 *    ##        ##   ##  ##     ##     ##  ##  ####  ##     ##
 *    ##         ## ##   ##     ##     ##  ##   ###  ##     ##
 *    ########    ###    ##     ##    #### ##    ## ####    ##
 */

if (ShardeumFlags.UseDBForAccounts === true) {
  AccountsStorage.init(config.server.baseDir, `${FilePaths.SHARDEUM_DB}`)
  if (isServiceMode()) AccountsStorage.lazyInit()
}

const appliedTxs = {} 
const shardusTxIdToEthTxId = {}

const defaultBalance = isDebugMode() ? oneSHM * BigInt(100) : BigInt(0)


const ERC20TokenBalanceMap: {
  to: string
  data: unknown
  timestamp: number
  result: unknown
}[] = []
const ERC20TokenCacheSize = 1000

interface RunStateWithLogs extends RunState {
  logs?: []
}

let EVM: { -readonly [P in keyof VM] }
let shardeumBlock: ShardeumBlock

let shardeumStateTXMap: Map<string, ShardeumState>

let shardusAddressToEVMAccountInfo: Map<string, EVMAccountInfo>

export let evmCommon

let debugAppdata: Map<string, unknown>

async function initEVMSingletons(): Promise<void> {
  const chainIDBN = BigInt(ShardeumFlags.ChainID)

  evmCommon = new Common({ chain: 'mainnet', hardfork: Hardfork.Istanbul, eips: [3855] })

  evmCommon.chainId = (): bigint => {
    return BigInt(chainIDBN.toString(10))
  }

  shardeumBlock = new ShardeumBlock({ common: evmCommon })

  if (ShardeumFlags.useShardeumVM) {
    const customEVM = new EthereumVirtualMachine({
      common: evmCommon,
      stateManager: undefined,
    })
    EVM = await VM.create({
      common: evmCommon,
      stateManager: undefined,
      evm: customEVM,
    })
  }

  shardeumStateTXMap = new Map<string, ShardeumState>()

  shardusAddressToEVMAccountInfo = new Map<string, EVMAccountInfo>()

  debugAppdata = new Map<string, unknown>()
}

initEVMSingletons()
```

<iframe width="800" height="480" src="https://www.youtube.com/embed/97yFJYDF9x8" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen id='svideo'></iframe>

