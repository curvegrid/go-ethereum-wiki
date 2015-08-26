This page describes how to set up a local cluster of nodes, advise how to make it private, and how to hook up your nodes on the eth-netstat network monitoring app. 
A fully controlled ethereum network is useful as a backend for network integration testing (core developers working on issues related to networking/blockchain synching/message propagation, etc or DAPP developers testing multi-block and multi-user scenarios).

We assume you are able to build `geth` following the [build instructions](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum)

## Setting up multiple nodes

In order to run multiple ethereum nodes locally, you have to make sure:
- each instance has a separate data directory
- each instance runs on a different port (both eth and rpc)
- the instances know about each other
- on Windows the ipc interface is either disabled or unique for each instance, in these instructions the ipc interface is disabled

You start the first node (let's make port explicit)
```bash
geth --datadir="/tmp/eth/60/01" -verbosity 6 --ipcdisable --port 30301 --rpcport 8101 console 2>> /tmp/eth/60/01.log
```

We started the node with the console, so that we can grab the enode for instance:

```
> admin.nodeInfo()
enode://8c544b4a07da02a9ee024def6f3ba24b2747272b64e16ec5dd6b17b55992f8980b77938155169d9d33807e501729ecb42f5c0a61018898c32799ced152e9f0d7@9[::]:30301
```

`[::]` will be parsed as localhost (`127.0.0.1`). If your nodes are on a local network check each individual host machine and find your ip with `ifconfig` (on Linux and MacOS):

```bash
$ ifconfig|grep netmask|awk '{print $2}'
127.0.0.1
192.168.1.97
```

If your peers are not on the local network, you need to know your external IP address (use a service) to construct the enode. If you want to connect to specific peer, enter the enodes in a `static-nodes.json` file. 

Now you can launch a second node with:

```bash
geth --datadir="/tmp/eth/60/02" --verbosity 6 --ipcdisable --port 30302 --rpcport 8102 console 2>> /tmp/eth/60/02.log 
```

You can test the connection  by typing in geth console:

```javascript
> net.listening
true
> net.peerCount 
1
> admin.peers
...
```

## Local cluster

As an extention of the above, you can spawn a local cluster of nodes easily. It can also be scripted including account creation which is needed for mining. 
See [`gethcluster.sh`](https://github.com/ethersphere/eth-utils) script, and the README there for usage and examples.

## Private network 

An ethereum network is a private network if the nodes are not connected to the main network nodes. So in this context private only means reserved or isolated, rather than protected or secure. Since connections are valid only if peers have identical protocol version and network id, you can effectively isolate your network by setting either of these to a non-canonical value. We recommend using the semantic `networkid` command line option for this. Its argument is an integer, the canonical network has id 0 (the default). And also allows you to work or mine on the same database as the live network.

## Monitoring your nodes

[This page](https://github.com/ethereum/wiki/wiki/Network-Status) describes how to use the [The Ethereum (centralised) network status monitor (known sometimes as "eth-netstats")](http://stats.ethdev.com) to monitor your nodes.

[This page](https://github.com/ethereum/go-ethereum/wiki/Setting-up-monitoring-on-local-cluster) or [this README](https://github.com/ethersphere/eth-utils) 
describes how you set up your own monitoring service for a (private or public) local cluster.
