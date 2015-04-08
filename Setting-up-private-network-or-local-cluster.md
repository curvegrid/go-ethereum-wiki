This page describes how to set up a local cluster of nodes, advise how to make it private, and how to hook up your nodes on the eth-netstat network monitoring app. 
We assume you are able to build `geth` following the [build instructions](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum)

## Setting up multiple nodes

In order to run multiple ethereum nodes locally, you have to make sure:
- each instance has a separate data directory
- each instance runs on a different port (both eth and rpc)
- the instances know about each other.

You start the first node (let's make port explicit)
```bash
geth --datadir="/tmp/eth/60/00" -loglevel 5 -logfile /tmp/eth/60/00.log -port 30300 -rpc -rpcport 8100  console
```

We started the node with the console, so that we can grab the enode for instance:

```
> admin.nodeInfo()
enode://8c544b4a07da02a9ee024def6f3ba24b2747272b64e16ec5dd6b17b55992f8980b77938155169d9d33807e501729ecb42f5c0a61018898c32799ced152e9f0d7@9[::]:30303
```

`[::]` will be parsed as localhost (`127.0.0.1`). If your nodes are on a local network check each individual host machine and find your ip with `ifconfig` (on Linux and MacOS):

```bash
$ ifconfig|grep netmask|awk '{print $2}'
127.0.0.1
192.168.1.97
```

Now you can launch a second node with:

```bash
geth --datadir="/tmp/eth/60/01" -loglevel 5 -logfile /tmp/eth/60/01.log -port 30301 -rpc -rpcport 8101 -bootnodes="enode://8c544b4a07da02a9ee024def6f3ba24b2747272b64e16ec5dd6b17b55992f8980b77938155169d9d33807e501729ecb42f5c0a61018898c32799ced152e9f0d7@127.0.0.1:30300" 
```

You can test the connection  by typing in geth console:

```javascript
> net
{
  listening: true,
  peerCount: 1
}
> admin.peers
...
```

## Local cluster

As an extention of the above, you can spawn a local cluster of nodes easily. It can also be scripted including account creation which is needed for mining. 
See [this script](https://gist.github.com/zelig/adcbb8cd4e9c8259327c)

Usage:

```
bash cluster <root> <n> <network_id> <runid> <local_IP> [[params]...]
```

This will set up a local cluster of nodes
- <n> is the number of clusters
- <root> is the root directory for the cluster, the nodes are set up 
  with datadir `<root>/00`, `<root>/01`, ...
- new accounts are created for each node
- they launch on port 30300, 30301, ...
- by collecting the nodes nodeUrl, they get connected to each other
- if enode has no IP, `<local_IP>` is substituted
- if `<network_id>` is not 0, they will not connect to a default client,
  resulting in a private isolated network
- the nodes log into `<root>/00.<runid>.log`, `<root>/01.<runid>.log`, ...
- `<runid>` is just an arbitrary tag or index you can use to log multiple 
  subsequent launches of the same cluster
- The nodes launch in mining mode 
- the cluster can be killed with `killall geth` (FIXME: should record PIDs)
  and restarted from the same state
- if you want to interact with the nodes, use rpc
- you can supply additional params on the command line which will be passed 
  to each node

## Private network 

An ethereum network is a private network if the nodes are not connected to the main network nodes. Since connections are valid only if peers have identical protocol version an network id, you can effectively isolate your network by setting either of these to a non-canonical value. We recommend using the semantic `networkid` command line option for this. Its argument is an integer, the cannonical network id is 0.

## Monitoring your nodes

[This page](https://github.com/ethereum/wiki/wiki/Network-Status) describes how to use the [The Ethereum (centralised) network status monitor (known sometimes as "eth-netstats")](http://eth-netstats.herokuapp.com) to monitor your nodes.

[This page](https://github.com/ethereum/go-ethereum/wiki/Setting-up-monitoring-on-local-cluster) describes how you set up your own monitoring service for a (private or public) local cluster.