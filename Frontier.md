**FRONTIER IS NOT YET LIVE**

Watch this: if issues for [Milestone Frontier](https://github.com/ethereum/go-ethereum/milestones) are not 100% closed, we are not ready for release.

Issues are tracked [on github](https://github.com/ethereum/go-ethereum/milestones/Frontier).

Frontier is the first in a series of releases. 

## Disclaimer

Frontier is a live testnet. It is here to help us prepare for the main release. 

* We fully expect instability and consensus flaws in the client, some of which may be exploitable
* As curators, we fully reserve the right to ignore blocks at our discretion
* As curators, from a final block that we solely determine, we will preserve all non-contract (i.e. code-less) account balances above the value of 1 ETH into the Homestead Genesis block

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

