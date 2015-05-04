By default, Geth will connect to public bootstrap nodes. This allows new users to hook into the network, find more peers, and download the current blockchain.

## Bootstrapping

By default, Geth will connect to public bootstrap nodes on startup. These public nodes give the client an entry point into the rest of the network and a way to discover additional peers.

To configure the bootnodes on startup, use the `--bootnodes` option and separate the peers by spaces. For example:
```
geth --bootnodes "enode://pubkey1@ip1:port1 enode://pubkey2@ip2:port2 enode://pubkey3@ip3:port3"
```

## Checking connectivity

To check how many peers the client is connected to in the interactive console, check the `net` object:
```
> net
{
  listening: true,
  peerCount: 7
}
```

To check the ports used by geth and also find your enode URI run:
```
> admin.nodeInfo()
{
  Name: 'Geth/v0.9.14/darwin/go1.4.2',
  NodeUrl: 'enode://3414c01c19aa75a34f2dbd2f8d0898dc79d6b219ad77f8155abf1a287ce2ba60f14998a3a98c0cf14915eabfdacf914a92b27a01769de18fa2d049dbf4c17694@[::]:30303',
  NodeID: '3414c01c19aa75a34f2dbd2f8d0898dc79d6b219ad77f8155abf1a287ce2ba60f14998a3a98c0cf14915eabfdacf914a92b27a01769de18fa2d049dbf4c17694',
  IP: '::',
  DiscPort: 30303,
  TCPPort: 30303,
  Td: '2044952618444',
  ListenAddr: '[::]:30303'
}
```

## Custom networks

Sometimes you might not need to connect to the live public network, you can instead choose to create your own private testnet. This is very useful if you don't need to test external contracts and want just to test the technology, because you won't have to compete with other miners and will easily generate a lot of test ether to play around (replace 12345 with any number):

```
geth -—networkid 12345 console
```
