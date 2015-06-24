**Please note, this is work in progress!**

# Overview
Beside the official [DApp API](https://github.com/ethereum/wiki/wiki/JSON-RPC) interface the go ethereum node has support for additional management API's. These API's are offered using [JSON-RPC](http://www.jsonrpc.org/specification) and follow the same conventions as used in the DApp API. The go ethereum package comes with a console client which has support for all additional API's.

# How to
It is possible to specify the set of API's which are offered over an interface with the `--${interface}api` command line argument for the go ethereum daemon. Where `${interface}` can be `rpc` for the `http` interface or `ipc` for an unix socket on unix or named pipe on Windows.

For example, `geth --ipcapi "admin,eth,miner" --rpcapi "eth,web3"` will
* enable the admin, official DApp and miner API over the IPC interface
* enable the eth and web3 API over the RPC interface

Please note that offering an API over the `rpc` interface will give everyone access to the API who can access this interface (e.g. DApp's). So be careful which API's you enable. By default geth enables all API's over the `ipc` interface and only the db,eth,net and web3 API's over the `rpc` interface.

To determine which API's an interface provides the `modules` transaction can be executes, e.g. over an `ipc` interface on unix systems:

```
echo '{"jsonrpc":"2.0","method":"modules","params":[],"id":1}' | nc -U $datadir/geth.ipc
```
will give all enabled modules including the version number:
```
{  
   "id":1,
   "jsonrpc":"2.0",
   "result":{  
      "admin":"1.0",
      "db":"1.0",
      "debug":"1.0",
      "eth":"1.0",
      "miner":"1.0",
      "net":"1.0",
      "personal":"1.0",
      "shh":"1.0",
      "txpool":"1.0",
      "web3":"1.0"
   }
}
```

## Integration
These additional API's follow the same conventions as the official DApp API. Web3 can be [extended](https://github.com/ethereum/web3.js/pull/229) and used to consume these additional API's. 

# API's
The different functions are split into multiple smaller logically grouped API's. Examples are given for the [Javascript console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console) but can easily be converted to a rpc request. 2 examples:

Console: `> miner.start()`

IPC: `echo '{"jsonrpc":"2.0","method":"miner_start","params":[],"id":1}' | nc -U $datadir/geth.ipc`

With the number of THREADS as an arguments:

Console: `> miner.start(4)`

IPC: `echo '{"jsonrpc":"2.0","method":"miner_start","params":[4],"id":1}' | nc -U $datadir/geth.ipc`

## Admin
Provides various function for managing a geth node
* [addPeer](#admin_addpeer)
* [peers] (#admin_peers)
* [importChain](#admin_importchain)
* [exportChain](#admin_exportchain)
* [verbosity](#admin_verbosity)
* [setSolc](#admin_setsolc)
* [startRPC](#admin_startrpc)
* [stopRPC](#admin_stoprpc)

## Db
This is the official DApp API. See for more information [this page](https://github.com/ethereum/wiki/wiki/JSON-RPC).

## Debug
* [dumpBlock](#debug_dumpblock)
* [getBlockRlp] (#debug_getblockrlp)
* [printBlock](#debug_printblock)
* [processBlock](#debug_processblock)
* [seedHash](#debug_seedhash)
* [setHead](#debug_sethead)

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

## Net
Network related functions
* [addPeer](#net_addpeer)
* [id](#net_id)
* [getPeerCount](#net_getpeercount)
* [getListening](#net_getlistening)
* [peers](#net_peers)

## Personal
Support for account management.
* [listAccounts](#personal_listaccounts)
* [newAccount](#personal_newaccount)
* [deleteAccount](#personal_deleteaccount)
* [unlockAccount](#personal_unlockaccount)

## Txpool
Gives insight in the transaction pool
* [status](#txpool_status)

## Web3
This is the official DApp API. See for more information [this page](https://github.com/ethereum/wiki/wiki/JSON-RPC).

### admin_addpeer
Add peer to node

#### Parameters
* `Url`, peer enode url

#### Return
`boolean` indicating if the peer was added

#### Example
`admin.addPeer("enode://4d19a2d...167fa41@XXX.XXX.XXX.XXX:30303")`
***

### admin_peers
This property will show all connected peers.

#### Example
`admin.peers`
***

### admin_importchain
Import an exported chain from file into node

#### Parameters
* `Filename`, the fully qualified path to the file containing the chain to be imported

#### Return
`boolean` indicating if chain was imported

#### Example
`admin.importChain("/tmp/chain.txt")`
***

### admin_exportchain
Export the blockchain to a file

#### Parameters
* `Filename`, the fully qualified path to the file where the blockchain must be exported

#### Return
`boolean` indicating if chain was exported

#### Example
`admin.exportChain("/tmp/chain.txt")`
***

### admin_verbosity
Set loglevel

#### Parameters
* `Level`, the verbosity level with 0 the least and 6 the most verbose

#### Return
`boolean` indicating if chain was exported

#### Example
`admin.verbosity(5)`
***

### admin_setSolc
Set the path to the solidity compiler for `eth.compileSolidity`.

#### Parameters
* `Path`, full path to solidity compiler

#### Return
`string` in case the path was valid a brief description about the solidity compiler

#### Example
`admin.setSolc("/tmp/solc")`
***

### admin_startrpc
Start the http RPC interface

#### Parameters
* `ListenAddress`, open listener on this host
* `ListenPort`, open listener on this port
* `CorsDomain`, the cross origin resource shared header
* `Apis`, comma separated list with the API modules which are offered over this interface

#### Return
`boolean` indication if the interface was started

#### Example
`admin.startRPC("127.0.0.1", 8545, "*", "eth,net,web3")`
***

### admin_stoprpc
Stop the http RPC interface

#### Return
`boolean` indication if the interface was stopped

#### Example
`admin.stopRPC()`

***

### debug_dumpblock
Dump block

#### Parameters
`integer`, block number

#### Return
`string` dumped block

#### Example
`debug.dumpBlock(0)`
***

### debug_getblockrlp
Get RLP encoded block

#### Parameters
`integer`, block number

#### Return
`string` RLP encoded block

#### Example
`debug.getBlockRlp(0)`
***

### debug_printblock
Pretty print block

#### Parameters
`integer`, block number

#### Return
`string` formatted block

#### Example
`debug.printBlock(0)`
***

### debug_processblock
Reprocess a block

#### Parameters
`integer`, block number

#### Return
`boolean` indication if the block was successful processed

#### Example
`debug.processBlock(0)`

***


### debug_seedhash
Block seed hash

#### Parameters
`NONE`

#### Return
`string` block seed hash

#### Example
`debug.seedHash(eth.blockNumber)`

***


### debug_sethead
Rewind the chain to a specific block

#### Parameters
`integer`, block number

#### Return
`boolean` indication if the new head was successful set

#### Example
`debug.setHead(eth.blockNumber-5000)`

***

### miner_start
This will generates the DAG if necessary and starts the miner

#### Parameters
`integer`, an optional integer which specifies the number of threads, if not specified the number of CPU's is used

#### Return
`boolean` indicating if the miner was started

#### Example
`miner.start()`
***

### miner_stop
This will stop the miner

#### Parameters
none

#### Return
`boolean` indicating if the miner was stopped

#### Example
`miner.stop()`

***

### miner_hashrate
Miner hashrate

#### Parameters
none

#### Return
`integer` hashes p/s

#### Example
`miner.hashrate`

***

### miner_setExtra
Store additional data in a mined block

#### Parameters
`string` string with extra data (max 1024 bytes)

#### Return
`boolean` indication if the DATA was set

***

### miner_setGasPrice
Set the gas price.

#### Parameters
`string` gas price, this can be a base8 (start with 0b), base10 (no prefix) or base16 representation (start with 0x)

#### Return
`boolean` indication if the new price was set

***

### miner_startAutoDAG
Pregenerate the DAG, this will allow for a seamless transition between the different epochs. If not enabled the miner will need to generate the DAG when a new epoch begins (each 30k blocks). This takes some time and will stop the miner until the DAG is generated.

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
`string`, peer enode

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
`array` collection of connected peers. 

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

### personal_listaccounts
List all accounts

#### Parameters
none

#### Return
`array` collection with accounts

#### Example
` personal.listAccounts`

***

### personal_newaccount
Create a new account

#### Parameters
`string`, passphrase to protect the account

#### Return
`string` address of the new account

#### Example
` personal.newAccount("mypasswd")`

***

### personal_deleteaccount
Delete an account

#### Parameters
`string`, address of the account to delete

`string`, passphrase of the account to delete

#### Return
`boolean` indication if the account was deleted

#### Example
` personal.deleteAccount(eth.coinbase, "mypasswd")`

***

### personal_unlockaccount
Unlock an account

#### Parameters
`string`, address of the account to delete

`string`, passphrase of the account to delete

`integer`, unlock account for duration seconds (optional)

#### Return
`boolean` indication if the account was unlocked

#### Example
` personal.unlockAccount(eth.coinbase, "mypasswd", 300)`

***

### txpool_status
Number of pending/queued transactions

#### Parameters
`NONE`

#### Return
`pending` all processable transactions

`queued` all non-processable transactions

#### Example
` txpool.status`

***