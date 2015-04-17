**FRONTIER IS NOT YET LIVE**

Watch this: if issues for [Milestone Frontier](https://github.com/ethereum/go-ethereum/milestones) are not 100% closed, we are not ready for release.

Issues are tracked [on github](https://github.com/ethereum/go-ethereum/milestones/Frontier).

# Geth

`geth` is the the command line interface for running a full ethereum node implemented in Go. 
It is the main deliverable of the [Frontier Release](https://github.com/ethereum/go-ethereum/wiki/Frontier)

## Capabilities

By installing and running `geth`, you can take part in the ethereum live testnet and
* mine real ether (at 10% of normal reward)
* transfer funds between addresses
* create contracts and send transactions
* explore block history
* and much much more

## Safety

Frontier is a live testnet. It is here to help us prepare for the main release.

* We fully expect instability and consensus flaws in the client, some of which may be exploitable
* As curators, we fully reserve the right to ignore blocks at our discretion
* As curators, from a final block that we solely determine, we will preserve all non-contract (i.e. code-less) account balances above the value of 1 ETH into the Homestead Genesis block

## Install 

Supported Platforms are Linux, Mac Os and Windows.

We support two types of installation: binary or scripted install for users. 
See [Install instructions](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum) for binary and scripted installs.

Developers and community enthusiast are advised to read the [Developers' Guide](https://github.com/ethereum/go-ethereum/wiki/Developers%27-Guide), which contains detailed instructions for manual build from source (on any platform) as well as detailed tips on testing, monitoring, contributing, debugging and submitting pull requests on github.

## Interfaces

* Javascript Console: `geth` can be launched with an interactive console, that provides a javascript runtime environment exposing a javascript API to interact with your node. [Javascript Console API](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console) includes the `web3` javascript Ðapp API as well as an additional admin API. 
* JSON-RPC server: `geth` can be launched with a json-rpc server that exposes the [JSON-RPC API](https://github.com/ethereum/wiki/wiki/JSON-RPC)
* [Command line options](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options) documents command line parameters as well as subcommands.

## Basic Use Case Documentation

* [Managing accounts](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)
* [Mining](https://github.com/ethereum/go-ethereum/wiki/mining)
* [Contracts and Transactions](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions)

**Note** buying and selling ether through exchanges is not discussed here. 

## License

The Ethereum Core Protocol licensed under the GNU Lesser General Public License. 
The go-ethereum client is licensed under the GPL.

## Troubleshooting

If something went wrong first read our [Troubleshooting](https://github.com/ethereum/go-ethereum/wiki/Troubleshooting) checklist as well as the [FAQ](https://github.com/ethereum/go-ethereum/wiki/Troubleshooting). If you still didn't find your answer please open an issue on GitHub or contact our help desk.

## Reporting 

Security issues are best sent to security@ethereum.org or shared in PM with devs on one of the channels.

Non-sensitive bug reports are welcome on github. Please always state the version (on master) or commit of your build (if on develop), give as much detail as possible about the situation and the anomaly that occurred. Provide logs or stacktrace if you can.

## Community and support

- IRC Freenode: #ethereum
- [Gitter](https://gitter.im/ethereum/go-ethereum)
- [Forum](https://forum.ethereum.org/categories/go-implementation)
- Helpdesk 

