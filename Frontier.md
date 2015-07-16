**FRONTIER IS NOT YET LIVE**

Watch this: if issues for [Milestone Frontier](https://github.com/ethereum/go-ethereum/milestones) are not 100% closed, we are not ready for release.

Issues are tracked [on github](https://github.com/ethereum/go-ethereum/milestones/Frontier).

## Disclaimer

Frontier is the first in a series of releases that punctuate the roadmap for the development of Ethereum. Ethereum is special and different from other software projects in that its release also involves launching the live network. After a year and a half of development the proof of concept series completed their 9 cycles. Iteration 10 (Proof of Concept series 9) led to the Olympic testnet, which gradually led to the release candidate client. 

The ethereum network goes live when the clients consent on the **genesis block** and start mining transactions on it. The genesis block will reference an initial system state where all the accounts set up by the presale exist with the correct amount of pre-issued ether allocated. Synchronised to the Frontier launch several exchanges start offering trade of ether allowing pre-allocated ether owners to sell their holdings as well as miners to exit with their earnings. 

During Frontier though some centralised kill switch functionality will still be in place as a contingency. These features will be removed in _Homestead_. As opposed to our earlier strategy, the decision is not to remove contracts from the blockchain. State in Homestead will be a direct and unmodified continuation of Frontier. 

Mining reward is the full amount of 5 ether per block (as opposed to our earlier proposal of a reduced amount). Mining rewards are discussed in detail [here](https://github.com/ethereum/go-ethereum/wiki/Mining#mining-rewards)

* We fully expect instability and consensus flaws in the client, some of which may be exploitable
* As curators, we fully reserve the right to ignore blocks at our discretion

## Components released

The focus of Frontier is the go implementation of an ethereum full node, with a command line interface codenamed `geth`. 

By [installing and running `geth`](https://github.com/ethereum/go-ethereum/wiki/Geth), you can take part in the ethereum live testnet, mine, transfer funds between addresses, create contracts and send transactions. 

**WARNING**: before you use `geth` or interact with the ethereum Frontier live testnet, make sure you read the official release notes and understand the caveats and risks. 

Apart from `geth` the go cli, the Frontier release contains the following ingredients:

* `web3.js`  library implementing the [JavaScript Dapp API](https://github.com/ethereum/wiki/wiki/JavaScript-API)
* `solc` a standalone solidity compiler. You only need this if you want to use your Dapp or [console to compile solidity code](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#compiling-a-contract).
* `ethminer` a standalone miner for openCL [GPU mining](https://github.com/ethereum/go-ethereum/wiki/Mining#gpu-mining)
* `netstat`  [network monitoring GUI](https://github.com/ethereum/wiki/wiki/Network-Status) is a `node.js` in-browser Dapp.

Resources: 
- [original announcement of the release scheme](https://blog.ethereum.org/2015/03/03/ethereum-launch-process)
- [follow-up blogpost](https://blog.ethereum.org/2015/03/12/getting-to-the-frontier/)
- [least authority audit](https://blog.ethereum.org/2015/07/07/know-ethereum-secure/), 
- 
