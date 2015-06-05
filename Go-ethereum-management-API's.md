**Please note, this is work in progress and not yet available!**

# Overview
Beside the official [DApp API](https://github.com/ethereum/wiki/wiki/JSON-RPC) interface the go ethereum node has support for additional management API's. These API's are offered using [JSON-RPC](http://www.jsonrpc.org/specification) and follow the same conventions as used in the DApp API. The go ethereum package comes with a console client which has support for all additional API's.

# How to
It is possible to specify the set of API's which are offered with the `--${interface}api` command line argument for the go ethereum daemon. Where `${interface}` can be `rpc` for the official DApp API or `ipc`.

For example, `geth --ipcapi "admin,eth,miner" --rpcapi "eth,web3"` will
* enable the admin, official DApp and miner API over the IPC interface
* enable the eth and web3 API over the official RPC interface

Please note that offering a API over the `rpc` interface will give everyone access to the admin interface who can access the `rpc` interface (e.g. DApp's). So be careful which API's are offered over an interface.

By default geth enables all API's over the `ipc` interface and only the eth and web3 API's over the official `rpc` interface.

## Integration
These additional API's follow the same conventions the official DApp API. Web3 can be [extended](https://github.com/ethereum/web3.js/pull/229) and used to consume these additional API's. 

# API's
The management functions are split into multiple smaller logically grouped API's.

## Admin

## Debug

## Eth
This is the official DApp API. See for more information [this page](https://github.com/ethereum/wiki/wiki/JSON-RPC).

## Miner
Allows full control over the miner and [DAG](https://github.com/ethereum/wiki/wiki/Ethash-DAG).
* [start](#miner_start)
* [stop](#miner_stop)
* [hashrate](#miner_hashrate)
* [setExtra](#miner_setextra)
* [setGasPrice](#miner_setgasprice)
* [startAutoDAG](#miner_startautodag)
* [stopAutoDAG](#miner_stopautodag)
* [makeDAG](#miner_makedag)

### Net
Network related functions
* [addPeer](#net_addpeer)
* [id](#net_id)
* [getPeerCount](#net_getpeercount)
* [getListening](#net_getlistening)
* [peers](#net_peers)

### Web3


### miner_start
This will generates the DAG if necessary and starts the miner

#### Parameters
* `THREADS`, an option integer which specifies the number of threads, if not specified the number of CPU's is used

#### Return
`boolean` indicating if the miner was started

***

### miner_stop
This will stop the miner

#### Parameters
none

#### Return
`boolean` indicating if the miner was stopped

***

### miner_hashrate
Miner hashrate

#### Parameters
none

#### Return
`integer` in hashes p/s

***

### miner_setExtra
Store additional data in a mined block

#### Parameters
`DATA` string with extra data (max 1024 bytes)

#### Return
`boolean` indication if the DATA was set

***

### miner_setGasPrice
Set the gas price.

#### Parameters
`Price` string with new price, this can be a base8 (start with 0b), base10 (no prefix) or base16 representation (start with 0x)

#### Return
`boolean` indication if the new price was set

***

### miner_startAutoDAG
Pregenerate the DAG, this will allow for a seamless transition between the different epochs. If not enables the miner will need to generate the DAG when a new epoch begins. This can take some time will the miner will need to wait for it to finish.

#### Parameters
none

#### Return
`boolean` indication if the command was successful

***

### miner_stopAutoDAG
Stop DAG pregeneration.

#### Parameters
none

#### Return
`boolean` indication if the command was successful

***

### miner_makeDAG
Start the DAG creator process.

#### Parameters
none

#### Return
`boolean` indication if the command was successful

***

### net_addpeer
Add peer

#### Parameters
`URL`, peer enode

#### Return
`boolean` indication if peer was added

***

### net_id
The network id (can be configured from the command line through the networkid argument)

#### Parameters
none

#### Return
`integer` network id

***

### net_getPeerCount
The number of connected peers

#### Parameters
none

#### Return
`integer` number of peers

***

### net_getListening
Indication if this node is currently listening for new peers

#### Parameters
none

#### Return
`boolean` indication if this node accepts new peers


***

### net_peers
Collection with peers

#### Parameters
none

#### Return
`Array` collection of connected peers. 

#### Example
```
> net.peers()
[{
  Caps: 'eth/60',
  ID: 'cc8125980267bbf5853848843520debaa05f7c66c83da0fe6599bd9f88ddff4c3c69a443e555873a966acfc59abe6ec6f707f6146886774d58a519eb074657f1',
  LocalAddress: '192.168.178.13:30303',
  Name: '++eth/v0.9.23/Release/Linux/g++/int',
  RemoteAddress: '81.241.47.56:38289'
}, {
  LocalAddress: '192.168.178.13:37063',
  Name: '++eth/v0.9.23/Release/Linux/g++/int',
  RemoteAddress: '54.72.239.134:30303',
  Caps: 'eth/60, eth/61',
  ID: '4a5a722a073c1c6356e7859c611260c36a1ac5815900dad21bf55a1014e314169d6156935722b1eb00212b795cf4c74d1167c571c0dfc3795d80b39ef45c1ef3'
}, {
  ID: 'e014ceab3d2fbe7215ca55b23e031cc031f46ca582a2a7741626d6189a08e40c77ddabb5970ad532970aa1b817b1428f76dc5454a0321d32a10d6d12efce5ded',
  LocalAddress: '192.168.178.13:30303',
  Name: '++eth/v0.9.23/Release/Linux/g++/int',
  RemoteAddress: '23.94.96.233:46512',
  Caps: 'eth/60, eth/61'
} ]
```