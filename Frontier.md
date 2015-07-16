**FRONTIER IS NOT YET LIVE**

Watch this: if issues for [Milestone Frontier](https://github.com/ethereum/go-ethereum/milestones) are not 100% closed, we are not ready for release.

Issues are tracked [on github](https://github.com/ethereum/go-ethereum/milestones/Frontier).

## Disclaimer

Frontier is the first in a series of releases that punctuate the roadmap for the development of Ethereum. Ethereum is special and different from other software projects in that its release also involves launching the live network. After a year and a half of development the proof of concept series completed their 9 cycles. Iteration 10 (Proof of Concept series 9) led to the Olympic testnet, which gradually led to the release candidate client. 

The ethereum network goes live when the clients consent on the **genesis block** and start mining transactions on it. The genesis block will reference an initial system state where all the accounts set up by the presale exist with the correct amount of pre-issued ether allocated. Synchronised to the Frontier launch several exchanges start offering trade of ether allowing pre-allocated ether owners to sell their holdings as well as miners to exit with their earnings. 

During Frontier though some centralised kill switch functionality will still be in place as a contingency measure. These features will be removed in _Homestead_. As opposed to our earlier strategy, the decision is not to remove any contracts from the blockchain and likewise leave all ether balances carry over to Homestead. In other words, the state in Homestead will be a direct and unmodified continuation of the state in Frontier. 

Mining reward is the full amount of 5 ether per block (as opposed to our earlier proposal of a reduced amount). Mining rewards are discussed in detail [here](https://github.com/ethereum/go-ethereum/wiki/Mining#mining-rewards)

* We fully expect instability and consensus flaws in the client, some of which may be exploitable
* As curators, we fully reserve the right to ignore blocks at our discretion

### SECURITY WARNINGS

* **You are responsible for your own computer security.** If your machine is compromised you **will** lose your ether, access to any contracts and maybe more.

* **You are responsible for your own actions.** If you mess something up or break any laws while using this software, it's your fault, and your fault only.

* **You are responsible for your own karma.** Don't be a jerk and respect others.

* This software is open source under a [GNU Lesser General Public License](https://www.gnu.org/licenses/lgpl-3.0.en.html) license.

### LEGAL WARNING: SHORT VERSION
#### Disclaimer of Liabilites and Warranties

* **The user expressly knows and agrees that the user is using the ethereum platform at the user’s sole risk.**
* **The user represents that the user has an adequate understanding of the risks, usage and intricacies of cryptographic tokens and blockchain-based open source software, eth platform and eth.** 
* **The user acknowledges and agrees that, to the fullest extent permitted by any applicable law, the disclaimers of liability contained herein apply to any and all damages or injury whatsoever caused by or related to risks of, use of, or inability to use, eth or the ethereum platform under any cause or action whatsoever of any kind in any jurisdiction, including, without limitation, actions for breach of warranty, breach of contract or tort (including negligence) and that neither stiftung ethereum nor the ethereum team shall be liable for any indirect, incidental, special, exemplary or consequential damages, including for loss of profits, goodwill or data.** 
* **Some jurisdictions do not allow the exclusion of certain warranties or the limitation or exclusion of liability for certain types of damages. therefore, some of the above limitations in this section may not apply to a user. In particular, nothing in these terms shall affect the statutory rights of any user or exclude injury arising from any willful misconduct or fraud of stiftung ethereum.**



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
- [The frontier website](https://frontier.ethereum.org)
- [Original announcement of the release scheme](https://blog.ethereum.org/2015/03/03/ethereum-launch-process) by Vinay Gupta
- [Follow-up blogpost](https://blog.ethereum.org/2015/03/12/getting-to-the-frontier/)
- [Least Authority audit blogpost](https://blog.ethereum.org/2015/07/07/know-ethereum-secure/) with links to the audit report, 
- [Olympic. Frontier prerelease](https://blog.ethereum.org/2015/05/09/olympic-frontier-pre-release/), Vitalik's blogpost detailing olympic rewards. 
