**FRONTIER IS COMING**

Watch this: if issues for [Milestone Frontier](https://github.com/ethereum/go-ethereum/milestones) are not 100% closed, we are not ready for release.

Issues are tracked [on github](https://github.com/ethereum/go-ethereum/milestones/Frontier).

## Introduction

Frontier is the first in a series of releases that punctuate the roadmap for the development of Ethereum. Frontier will be followed by ‘Homestead’, ‘Metropolis’ and ‘Serenity’ throughout the coming year, each adding new features and improving the user friendliness and security of the platform. 

Ethereum is special and different from other software projects in that its release also involves launching the live network. After a year and a half of development the proof of concept series completed their 9 cycles. Iteration 10 (Proof of Concept series 9) led to the Olympic testnet, which gradually led to the release candidate client for Frontier. 

The ethereum network goes live when the clients consent on the **genesis block** and start mining transactions on it. The genesis block will reference an initial system state where all the accounts set up by the presale exist with the correct amount of pre-issued ether allocated. Synchronised to the Frontier launch several exchanges start offering trade of ether allowing pre-allocated ether owners to sell their holdings as well as miners to exit with their earnings. 

During Frontier though some centralised kill switch functionality will still be in place as a contingency measure. These features will be removed in _Homestead_. As opposed to our earlier strategy, the decision is not to remove any contracts from the blockchain and likewise leave all ether balances carry over to Homestead. In other words, the state in Homestead will be a direct and unmodified continuation of the state in Frontier. 

Mining reward is the full amount of 5 ether per block (as opposed to our earlier proposal of a reduced amount). Mining rewards are discussed in detail [here](https://github.com/ethereum/go-ethereum/wiki/Mining#mining-rewards)

## Safety warnings


* **You are responsible for your own computer security.** If your machine is compromised you **will** lose your ether, access to any contracts and maybe more. 
* **You are responsible for your own actions.** If you mess something up or break any laws while using this software, it's your fault, and your fault only.
* **You are responsible for your own karma.** Don't be a jerk and respect others.

**WARNING:** Before you interact with the ethereum Frontier live testnet, make sure you read the documentation and understand the caveats and risks. Please read the [legal disclaimer](https://github.com/ethereum/go-ethereum/wiki/Disclaimer)

## Components released

The focus of Frontier is the go implementation of an ethereum full node, with a command line interface codenamed `geth`. 

By [installing and running `geth`](https://github.com/ethereum/go-ethereum/wiki/Geth), you can take part in the ethereum live testnet, mine, transfer funds between addresses, create contracts and send transactions. 

**WARNING**: before you use `geth` or interact with the ethereum Frontier live testnet, make sure you read the documentation and fully understand the [caveats and risks](https://github.com/ethereum/go-ethereum/wiki/Disclaimer). 

Apart from `geth` the go cli, the Frontier release contains the following ingredients:

* `web3.js`  library implementing the [JavaScript Dapp API](https://github.com/ethereum/wiki/wiki/JavaScript-API)
* `solc` a standalone solidity compiler. You only need this if you want to use your Dapp or [console to compile solidity code](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#compiling-a-contract).
* `ethminer` a standalone miner for openCL [GPU mining](https://github.com/ethereum/go-ethereum/wiki/Mining#gpu-mining)
* `netstat`  [network monitoring GUI](https://github.com/ethereum/wiki/wiki/Network-Status) is a `node.js` in-browser Dapp.

## The actual launch process 

Ethereum is not something that’s centrally ‘launched’, but instead emerges from consensus. Users will have to voluntarily download and run a specific version of the software, then manually generate and load the Genesis block to join the official project’s network.

Once Frontier has been installed on their machines, users will need to generate the Genesis block themselves, then load it into their Frontier clients. A script and instructions on how to do this will be provided as part of the new Ethereum website, as well as our various wikis.

We’re often asked how existing users will switch from the test network to the live network: it will be done through a switch at the geth console (--networkId). By default the new build will aim to connect to the live network, to switch back to the test network you’ll simply indicate a network id of ‘0’.

## Bugs, Issues and Complications

The work on the Frontier software is far from over. Expect weekly updates which will give you access to better, more stable clients. Many of the planned Frontier gotchas (which included a chain reset at Homestead, limiting mining rewards to 10%, and centralized checkpointing) were deemed unnecessary. However, there are still big differences between Frontier and Homestead. In Frontier, we’re going to have issues, we’re going to have updates, and there will be bugs – users are taking their chances when using the software. There will be big (BIG) warning messages before developers are able to even install it. In Frontier, documentation is limited, and the tools provided require advanced technical skills.

## The Canary Contracts

The Canary contracts are simple switches holding a value equal to 0 or 1. Each contract is controlled by a different member of the Eth/Dev team and will be updated to ‘1’ if the internal Frontier Disaster Recovery Team flags up a consensus issue, such as a fork.

Within each Frontier client, a check is made after each block against 4 contracts. If two out of four of these contracts have a value switched from 0 to 1, mining stops and a message urging the user to update their client is displayed. This is to prevent “fire and forget” miners from preventing a chain upgrade.

This process is centralized and will only run for the duration of Frontier. It helps preventing the risk of a prolonged period (24h+) of outage.

## Stats, Status and Badblock websites

You probably are already familiar with our network stats monitor, https://stats.ethdev.com/. It gives a quick overview of the health of the network, block resolution time and Gas statistics. If you’d like to explore it further, I’ve made a brief video explaining the various KPIs. Remember that participation in the stats page is voluntary, and nodes have to add themselves before they appear on the panel.

In addition to the stats page, we will have a status page at https://status.ethdev.com/ (no link as the site is not live yet) which will gives a concise overview of any issue that might be affecting Frontier. Use it as your first port of call if you think something might not be right.

Finally, if any of the clients receive an invalid block, they will refuse to process it send it to the bad block website (AKA ‘Sentinel’). This could mean a bug, or something more serious, such as a fork. Either way, this process will alert our developers to potential issues on the network. The website itself is public and available at https://badblocks.ethdev.com (currently operating on the testnet).
A Clean Testnet

During the last couple of months, the Ethereum test network was pushed to its limits in order to test scalability and block propagation times. As part of this test we encouraged users to spam the network with transactions, contract creation code and call to contracts, at times reaching over 25 transactions per second. This has led the test network chain to grow to a rather unwieldy size, making it difficult for new users to catch up. For this reason, and shortly after the Frontier release, there will be a new test network following the same rules as Frontier.

## Olympic rewards distribution

During the Olympic phase there were a number of rewards for various achievements including mining prowess. These rewards will not be part of the Frontier Genesis block, but instead will be handed out by a Foundation bot during the weeks following the release.

Resources: 
- [Frontier is coming](https://blog.ethereum.org/2015/07/22/frontier-is-coming-what-to-expect-and-how-to-prepare/) blogpost by Stephan Tual announcing the launch. 
- [The frontier website](https://frontier.ethereum.org)
- [Original announcement of the release scheme](https://blog.ethereum.org/2015/03/03/ethereum-launch-process) by Vinay Gupta
- [Follow-up blogpost](https://blog.ethereum.org/2015/03/12/getting-to-the-frontier/)
- [Least Authority audit blogpost](https://blog.ethereum.org/2015/07/07/know-ethereum-secure/) with links to the audit report, 
- [Olympic. Frontier prerelease](https://blog.ethereum.org/2015/05/09/olympic-frontier-pre-release/), Vitalik's blogpost detailing olympic rewards. 
