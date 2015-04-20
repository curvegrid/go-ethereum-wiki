* [So what is mining anyway?](#sowhatismininganyway)
* [Mining with geth](#miningwithgeth)
* [GPU mining](#gpumining)
* [Resources](#resources)

## So what is mining anyway?

Ethereum Frontier like all blockchain technologies uses an incentive-driven model of security. Consensus is based on choosing the block with the highest total difficulty. 
Miners produce blocks which the others check for validity. Among other well-formedness criteria, a block is only valid if it contains **proof of work** (PoW) of a given **difficulty**. 
Note that in Ethereum 1.1, this is likely gonna be replaced by a **proof of stake** model.

[The proof of work algorithm used is called [Ethash](https://github.com/ethereum/wiki/wiki/Ethash) (a modified version of [Dagger-Hashimoto](https://github.com/ethereum/wiki/wiki/Dagger-Hashimoto) involves finding a nonce input to the algorithm so that the result is below a certain threshold depending on the difficulty. The point in PoW algorithms is that there is no better strategy to find such a nonce than enumerating the possibilities while verification of a solution is trivial and cheap. If outputs have a uniform distribution, then we can guarantee that on average the time needed to find a nonce depends on the difficulty threshold, making it possible to control the time of finding a new block just by manipulating difficulty.

The difficulty dynamically adjusts so that on average one block is produced by the entire network every 12 seconds (ie., 12 s block time). This heartbeat basically punctuates the synchronisation of system state and guarantees that maintaining a fork (to allow double spend) or rewriting history is impossible unless the attacker possesses more than half of the network mining power (so called 51% attack).

Any node participating in the network can be a miner and their expected revenue from mining will be directly proportional to their (relative) mining power or **hashrate**, ie., number of nonces tried per second normalised by the total hashrate of the network.

Ethash PoW is memory hard, making it basically ASIC resistant. This basically means that calculating the PoW requires choosing subsets of a fixed resource dependent on the nonce and block header. This resource (a few gigabyte size data) is called a **DAG**. The [DAG](https://github.com/ethereum/wiki/wiki/Ethash-DAG) is totally different every 30000 blocks (a 100 hour window, called an **epoch**) and takes a while to generate. Since the DAG only depends on block height, it can be pregenerated but if its not, the client need to wait the end of this process to produce a block. Until clients actually precache dags ahead of time the network may experience a massive block delay on each epoch transition. Note that the DAG does not need to be generated for verifying the PoW essentially allowing for verifiction with both low CPU and small memory.

As a special case, when you start up your node from scratch, mining will only start once the DAG is built for the current epoch. 

## Mining with Geth

When you start up your ethereum node with `geth` it is not mining by default. To start it in mining mode, you use the `-mine` [command line option](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options). The `-minerthreads` parameter can be used to set the number parallel mining threads (defaulting to the total number of processor cores). 

     geth -mine -minerthreads=4

You can also start and stop mining at runtime using the [console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminerstart). 

```
> admin.miner.start()
true
> admin.miner.stop()
true
```

In order to earn ether through you need to have a **coinbase** (or **etherbase**) address set. This etherbase defaults to your [primary account](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts). Note that miner will not complain if you got no etherbase address set, however money then will be sent to a zero address meaning you are not gonna receive the mining reward. 

```
> eth.coinbase
'0x'
> admin.newAccount()
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
'ffd25e388bf07765e6d7a00d6ae83fa750460c7e'
> eth.coinbase
'0xffd25e388bf07765e6d7a00d6ae83fa750460c7e'```
```

Note that your coinbase does not need to be an address of a local account, just an existing one. 

```
admin.accounts
[]
eth.coinbase = 'a4d8e9cae4d04b093aac82e6cd355b6b963fb7ff'
```

There is an option [to add extra Data](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminersetextra) (up to the limit of 1Kb) to your mined blocks. By convention this is interpreted as a unicode string, so you can set your vanity tag.

```
admin.miner.setExtra("ΞTHΞЯSPHΞЯΞ")
...
admin.debug.printBlock(131805)
BLOCK(be465b020fdbedc4063756f0912b5a89bbb4735bd1d1df84363e05ade0195cb1): Size: 531.00 B TD: 643485290485 {
NoNonce: ee48752c3a0bfe3d85339451a5f3f411c21c8170353e450985e1faab0a9ac4cc
Header:
[
...
        Coinbase:           a4d8e9cae4d04b093aac82e6cd355b6b963fb7ff
        Number:             131805
        Extra:              ΞTHΞЯSPHΞЯΞ
...
}
```

You can check your hashrate with [admin.miner.hashrate](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminerhashrate) , the result is in KH/s (1000 Hash operations per second). 

```
> admin.miner.hashrate()
712
```

After you successfully mined some blocks, you can check the ether balance of your coinbase account. Now assuming your coinbase is a local account:

```
> eth.getBalance(eth.coinbase).toNumber();
'34698870000000' 
```

In order to spend your earnings [on gas to transact](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions), you will need to have this account unlocked. 

```
> admin.unlock(eth.coinbase)
Please enter a passphrase now.
Passphrase:
true
```

## GPU mining

**TODO**
Using a graphic card processor chip for mining.

## Resources:

* https://blog.ethereum.org/2014/07/05/stake/
* https://blog.ethereum.org/2014/10/03/slasher-ghost-developments-proof-stake/
* https://blog.ethereum.org/2014/06/19/mining/
* https://github.com/ethereum/wiki/wiki/Dagger-Hashimoto
* https://github.com/ethereum/wiki/wiki/Ethash


