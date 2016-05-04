Beside the official [DApp APIs](https://github.com/ethereum/wiki/wiki/JSON-RPC) interface go-ethereum
has support for additional management APIs. Similar to the DApp APIs, these are also provided using
[JSON-RPC](http://www.jsonrpc.org/specification) and follow exactly the same conventions. Geth comes
with a console client which has support for all additional APIs described here.

## Enabling the management APIs

To offer these APIs over the Geth RPC endpoints, please specify them with the `--${interface}api`
command line argument (where `${interface}` can be `rpc` for the HTTP endpoint, `ws` for the WebSocket
endpoint and `ipc` for the unix socket (Unix) or named pipe (Windows) endpoint).

For example: `geth --ipcapi admin,eth,miner --rpcapi eth,web3 --rpc`

* Enables the admin, official DApp and miner API over the IPC interface
* Enables the official DApp and web3 API over the HTTP interface

The HTTP RPC interface must be explicitly enabled using the `--rpc` flag.

Please note, offering an API over the HTTP (`rpc`) or WebSocket (`ws`) interfaces will give everyone
access to the APIs who can access this interface (DApps, browser tabs, etc). Be careful which APIs
you enable. By default Geth enables all APIs over the IPC (`ipc`) interface and only the `db`, `eth`,
`net` and `web3` APIs over the HTTP and WebSocket interfaces.

To determine which APIs an interface provides, the `modules` JSON-RPC method can be invoked. For
example over an `ipc` interface on unix systems:

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

## Consuming the management APIs

These additional APIs follow the same conventions as the official DApp APIs. Web3 can be
[extended](https://github.com/ethereum/web3.js/pull/229) and used to consume these additional APIs. 

The different functions are split into multiple smaller logically grouped APIs. Examples are given
for the [JavaScript console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console) but
can easily be converted to an RPC request.

**2 examples:**

* Console: `miner.start()`

* IPC: `echo '{"jsonrpc":"2.0","method":"miner_start","params":[],"id":1}' | nc -U $datadir/geth.ipc`

* HTTP: `curl -X POST --data '{"jsonrpc":"2.0","method":"miner_start","params":[],"id":74}' localhost:8545`

With the number of THREADS as an arguments:

* Console: `miner.start(4)`

* IPC: `echo '{"jsonrpc":"2.0","method":"miner_start","params":[4],"id":1}' | nc -U $datadir/geth.ipc`

* HTTP: `curl -X POST --data '{"jsonrpc":"2.0","method":"miner_start","params":[4],"id":74}' localhost:8545`

## List of management APIs

Beside the officially exposed DApp API namespaces (`eth`, `shh`, `web3`), Geth provides the following
extra management API namespaces:

* `admin`: Geth node management
* `debug`: Geth node debugging
* `miner`: Miner and [DAG](https://github.com/ethereum/wiki/wiki/Ethash-DAG) management
* `personal`: Account management
* `txpool`: Transaction pool inspection

| admin                             | [debug](#debug)                                 | miner                               | personal                                 | txpool                   |
|:---------------------------------:|:-----------------------------------------------:|:-----------------------------------:|:----------------------------------------:|:------------------------:|
| [addPeer](#admin_addpeer)         | [dumpBlock](#debug_dumpblock)                   | [start](#miner_start)               | [listAccounts](#personal_listaccounts)   | [status](#txpool_status) |
| [datadir](#datadir)               | [getBlockRlp](#debug_getblockrlp)               | [stop](#miner_stop)                 | [newAccount](#personal_newaccount)       |                          |
| [nodeInfo](#admin_nodeinfo)       | [printBlock](#debug_printblock)                 | [hashrate](#miner_hashrate)         | [unlockAccount](#personal_unlockaccount) |                          |
| [peers](#admin_peers)             | [processBlock](#debug_processblock)             | [setExtra](#miner_setextra)         |                                          |                          |
| [setSolc](#admin_setcolc)         | [seedHash](#debug_seedhash)                     | [setGasPrice](#miner_setgasprice)   |                                          |                          |
| [startRPC](#admin_startrpc)       | [setHead](#debug_sethead)                       | [startAutoDAG](#miner_startautodag) |                                          |                          |
| [startWS](#admin_startws)         | [traceTransaction](#debug_tracetrancaction)     | [stopAutoDAG](#miner_stopAutodag)   |                                          |                          |
| [stopRPC](#admin_stoprpc)         | [traceBlock](#debug_traceblock)                 | [makeDAG](#miner_makedag)           |                                          |                          |
| [stopWS](#admin_stopws)           | [traceBlockFromFile](#debug_traceblockfromfile) |                                     |                                          |                          |
|                                   | [traceBlockByNumber](#debug_blockbynumber)      |                                     |                                          |                          |

## Admin

The `admin` API gives you access to several non-standard RPC methods, which will allow you to have
a fine grained control over your Geth instance, including but not limited to network peer and RPC
endpoint management.

### admin_addPeer

The `addPeer` administrative method requests adding a new remote node to the list of tracked static
nodes. The node will try to maintain connectivity to these nodes at all times, reconnecting every
once in a while if the remote connection goes down.

The method accepts a single argument, the [`enode`](https://github.com/ethereum/wiki/wiki/enode-url-format)
URL of the remote peer to start tracking and returns a `BOOL` indicating whether the peer was accepted
for tracking or some error occurred.

| Client  | Method invocation                              |
|:-------:|------------------------------------------------|
| Go      | `admin.AddPeer(url string) (bool, error)`      |
| Console | `admin.addPeer(url)`                           |
| RPC     | `{"method": "admin_addPeer", "params": [url]}` |

#### Example

```javascript
> admin.addPeer("enode://a979fb575495b8d6db44f750317d0f4622bf4c2aa3365d6af7c284339968eef29b69ad0dce72a4d8db5ebb4968de0e3bec910127f134779fbcb0cb6d3331163c@52.16.188.185:30303")
true
```

### admin_datadir

The `datadir` administrative property can be queried for the absolute path the running Geth node
currently uses to store all its databases.

| Client  | Method invocation                 |
|:-------:|-----------------------------------|
| Go      | `admin.Datadir() (string, error`) |
| Console | `admin.datadir`                   |
| RPC     | `{"method": "admin_datadir"}`     |

#### Example

```javascript
> admin.datadir
"/home/karalabe/.ethereum"
```

### admin_nodeInfo

The `nodeInfo` administrative property can be queried for all the information known about the running
Geth node at the networking granularity. These include general information about the node itself as a
participant of the [ÐΞVp2p](https://github.com/ethereum/wiki/wiki/%C3%90%CE%9EVp2p-Wire-Protocol) P2P
overlay protocol, as well as specialized information added by each of the running application protocols
(e.g. `eth`, `les`, `shh`, `bzz`).

| Client  | Method invocation                         |
|:-------:|-------------------------------------------|
| Go      | `admin.NodeInfo() (*p2p.NodeInfo, error`) |
| Console | `admin.nodeInfo`                          |
| RPC     | `{"method": "admin_nodeInfo"}`            |

#### Example

```javascript
> admin.nodeInfo
{
  enode: "enode://44826a5d6a55f88a18298bca4773fca5749cdc3a5c9f308aa7d810e9b31123f3e7c5fba0b1d70aac5308426f47df2a128a6747040a3815cc7dd7167d03be320d@[::]:30303",
  id: "44826a5d6a55f88a18298bca4773fca5749cdc3a5c9f308aa7d810e9b31123f3e7c5fba0b1d70aac5308426f47df2a128a6747040a3815cc7dd7167d03be320d",
  ip: "::",
  listenAddr: "[::]:30303",
  name: "Geth/v1.5.0-unstable/linux/go1.6",
  ports: {
    discovery: 30303,
    listener: 30303
  },
  protocols: {
    eth: {
      difficulty: 17334254859343145000,
      genesis: "0xd4e56740f876aef8c010b86a40d5f56745a118d0906a34e69aec8c0db1cb8fa3",
      head: "0xb83f73fbe6220c111136aefd27b160bf4a34085c65ba89f24246b3162257c36a",
      network: 1
    }
  }
}
```

### admin_peers

The `peers` administrative property can be queried for all the information known about the connected
remote nodes at the networking granularity. These include general information about the nodes themselves
as participants of the [ÐΞVp2p](https://github.com/ethereum/wiki/wiki/%C3%90%CE%9EVp2p-Wire-Protocol)
P2P overlay protocol, as well as specialized information added by each of the running application
protocols (e.g. `eth`, `les`, `shh`, `bzz`).

| Client  | Method invocation                        |
|:-------:|------------------------------------------|
| Go      | `admin.Peers() ([]*p2p.PeerInfo, error`) |
| Console | `admin.peers`                            |
| RPC     | `{"method": "admin_peers"}`              |

#### Example

```javascript
> admin.peers
[{
    caps: ["eth/61", "eth/62", "eth/63"],
    id: "08a6b39263470c78d3e4f58e3c997cd2e7af623afce64656cfc56480babcea7a9138f3d09d7b9879344c2d2e457679e3655d4b56eaff5fd4fd7f147bdb045124",
    name: "Geth/v1.5.0-unstable/linux/go1.5.1",
    network: {
      localAddress: "192.168.0.104:51068",
      remoteAddress: "71.62.31.72:30303"
    },
    protocols: {
      eth: {
        difficulty: 17334052235346465000,
        head: "5794b768dae6c6ee5366e6ca7662bdff2882576e09609bf778633e470e0e7852",
        version: 63
      }
    }
}, /* ... */ {
    caps: ["eth/61", "eth/62", "eth/63"],
    id: "fcad9f6d3faf89a0908a11ddae9d4be3a1039108263b06c96171eb3b0f3ba85a7095a03bb65198c35a04829032d198759edfca9b63a8b69dc47a205d94fce7cc",
    name: "Geth/v1.3.5-506c9277/linux/go1.4.2",
    network: {
      localAddress: "192.168.0.104:55968",
      remoteAddress: "121.196.232.205:30303"
    },
    protocols: {
      eth: {
        difficulty: 17335165914080772000,
        head: "5794b768dae6c6ee5366e6ca7662bdff2882576e09609bf778633e470e0e7852",
        version: 63
      }
    }
}]
```

### admin_setSolc

The `setSolc` administrative method sets the Solidity compiler path to be used by the node when
invoking the `eth_compileSolidity` RPC method. The Solidity compiler path defaults to `/usr/bin/solc`
if not set, so you only need to change it for using a non-standard compiler location.

The method accepts an absolute path to the Solidity compiler to use (specifying a relative path
would depend on the current – to the user unknown – working directory of Geth), and returns the
version string reported by `solc --version`.

| Client  | Method invocation                               |
|:-------:|-------------------------------------------------|
| Go      | `admin.SetSolc(path string) (string, error`)    |
| Console | `admin.setSolc(path)`                           |
| RPC     | `{"method": "admin_setSolc", "params": [path]}` |

#### Example

```javascript
> admin.setSolc("/usr/bin/solc")
"solc, the solidity compiler commandline interface\nVersion: 0.3.2-0/Release-Linux/g++/Interpreter\n\npath: /usr/bin/solc"
```

### admin_startRPC

The `startRPC` administrative method starts an HTTP based [JSON RPC](http://www.jsonrpc.org/specification)
API webserver to handle client requests. All the parameters are optional:

* `host`: network interface to open the listener socket on (defaults to `"localhost"`)
* `port`: network port to open the listener socket on (defaults to `8545`)
* `cors`: [cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) header to use (defaults to `""`)
* `apis`: API modules to offer over this interface (defaults to `"eth,net,web3"`)

The method returns a boolean flag specifying whether the HTTP RPC listener was opened or not. Please note, only one HTTP endpoint is allowed to be active at any time.

| Client  | Method invocation                                                                             |
|:-------:|-----------------------------------------------------------------------------------------------|
| Go      | `admin.StartRPC(host *string, port *rpc.HexNumber, cors *string, apis *string) (bool, error)` |
| Console | `admin.startRPC(host, port, cors, apis)`                                                      |
| RPC     | `{"method": "admin_startRPC", "params": [host, port, cors, apis]}`                            |

#### Example

```javascript
> admin.startRPC("127.0.0.1", 8545)
true
```

### admin_startWS

The `startWS` administrative method starts an WebSocket based [JSON RPC](http://www.jsonrpc.org/specification)
API webserver to handle client requests. All the parameters are optional:

* `host`: network interface to open the listener socket on (defaults to `"localhost"`)
* `port`: network port to open the listener socket on (defaults to `8546`)
* `cors`: [cross-origin resource sharing](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) header to use (defaults to `""`)
* `apis`: API modules to offer over this interface (defaults to `"eth,net,web3"`)

The method returns a boolean flag specifying whether the WebSocket RPC listener was opened or not. Please note, only one WebSocket endpoint is allowed to be active at any time.

| Client  | Method invocation                                                                             |
|:-------:|-----------------------------------------------------------------------------------------------|
| Go      | `admin.StartWS(host *string, port *rpc.HexNumber, cors *string, apis *string) (bool, error)` |
| Console | `admin.startWS(host, port, cors, apis)`                                                      |
| RPC     | `{"method": "admin_startWS", "params": [host, port, cors, apis]}`                            |

#### Example

```javascript
> admin.startWS("127.0.0.1", 8546)
true
```

### admin_stopRPC

The `stopRPC` administrative method closes the currently open HTTP RPC endpoint. As the node can only have a single HTTP endpoint running, this method takes no parameters, returning a boolean whether the endpoint was closed or not.

| Client  | Method invocation               |
|:-------:|---------------------------------|
| Go      | `admin.StopRPC() (bool, error`) |
| Console | `admin.stopRPC()`               |
| RPC     | `{"method": "admin_stopRPC"`    |

#### Example

```javascript
> admin.stopRPC()
true
```

### admin_stopWS

The `stopWS` administrative method closes the currently open WebSocket RPC endpoint. As the node can only have a single WebSocket endpoint running, this method takes no parameters, returning a boolean whether the endpoint was closed or not.

| Client  | Method invocation              |
|:-------:|--------------------------------|
| Go      | `admin.StopWS() (bool, error`) |
| Console | `admin.stopWS()`               |
| RPC     | `{"method": "admin_stopWS"`    |

#### Example

```javascript
> admin.stopWS()
true
```

## Debug

The `debug` API gives you access to several non-standard RPC methods, which will allow you to inspect,
debug and set certain debugging flags during runtime.

### debug_traceTransaction

The `traceTransaction` debugging method will attempt to run the transaction in the exact same manner
as it was executed on the network. It will replay any transaction that may have been executed prior
to this one before it will finally attempt to execute the transaction that corresponds to the given
hash.

In addition to the hash of the transaction you may give it a secondary *optional* argument, which
specifies the options for this specific call. The possible options are:

* `disableStorage`: `BOOL`. Setting this to true will disable storage capture (default = false).
* `disableMemory`: `BOOL`. Setting this to true will disable memory capture (default = false).
* `disableStack`: `BOOL`. Setting this to true will disable stack capture (default = false).
* `fullStorage`: `BOOL`. Setting this to true will return you, for each opcode, the full storage,
    including everything which hasn't changed. This is a slow process and is therefor defaulted to `false`. By default it will only ever give you the changed storage values.

| Client  | Method invocation                                                                            |
|:-------:|----------------------------------------------------------------------------------------------|
| Go      | `debug.TraceTransaction(txHash common.Hash, logger *vm.LogConfig) (*ExecutionResurt, error)` |
| Console | `debug.traceTransaction(txHash, [options])`                                                  |
| RPC     | `{"method": "debug_traceTransaction", "params": [txHash, {}]}`                               |


#### Example

```javascript
> debug.traceTransaction("0x2059dd53ecac9827faad14d364f9e04b1d5fe5b506e3acc886eff7a6f88a696a")
{
  gas: 85301,
  returnValue: "",
  structLogs: [{
      depth: 1,
      error: "",
      gas: 162106,
      gasCost: 3,
      memory: null,
      op: "PUSH1",
      pc: 0,
      stack: [],
      storage: {}
  },
    /* snip */
  {
      depth: 1,
      error: "",
      gas: 100000,
      gasCost: 0,
      memory: ["0000000000000000000000000000000000000000000000000000000000000006", "0000000000000000000000000000000000000000000000000000000000000000", "0000000000000000000000000000000000000000000000000000000000000060"],
      op: "STOP",
      pc: 120,
      stack: ["00000000000000000000000000000000000000000000000000000000d67cbec9"],
      storage: {
        0000000000000000000000000000000000000000000000000000000000000004: "8241fa522772837f0d05511f20caa6da1d5a3209000000000000000400000001",
        0000000000000000000000000000000000000000000000000000000000000006: "0000000000000000000000000000000000000000000000000000000000000001",
        f652222313e28459528d920b65115c16c04f3efc82aaedc97be59f3f377c0d3f: "00000000000000000000000002e816afc1b5c0f39852131959d946eb3b07b5ad"
      }
  }]
```

### debug_traceBlock

The `traceBlock` method will return a full stack trace of all invoked opcodes of all transaction
that were included included in this block. **Note**, the parent of this block must be present or
it will fail.

| Client  | Method invocation                                                        |
|:-------:|--------------------------------------------------------------------------|
| Go      | `debug.TraceBlock(blockRlp []byte, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlock(tblockRlp, [options])`                                 |
| RPC     | `{"method": "debug_traceBlock", "params": [blockRlp, {}]}`               |

References:
[RLP](https://github.com/ethereum/wiki/wiki/RLP)

#### Example

```javascript
> debug.traceBlock("0xblock_rlp")
{
  gas: 85301,
  returnValue: "",
  structLogs: [{
      depth: 1,
      error: "",
      gas: 162106,
      gasCost: 3,
      memory: null,
      op: "PUSH1",
      pc: 0,
      stack: [],
      storage: {}
  },
    /* snip */
  {
      depth: 1,
      error: "",
      gas: 100000,
      gasCost: 0,
      memory: ["0000000000000000000000000000000000000000000000000000000000000006", "0000000000000000000000000000000000000000000000000000000000000000", "0000000000000000000000000000000000000000000000000000000000000060"],
      op: "STOP",
      pc: 120,
      stack: ["00000000000000000000000000000000000000000000000000000000d67cbec9"],
      storage: {
        0000000000000000000000000000000000000000000000000000000000000004: "8241fa522772837f0d05511f20caa6da1d5a3209000000000000000400000001",
        0000000000000000000000000000000000000000000000000000000000000006: "0000000000000000000000000000000000000000000000000000000000000001",
        f652222313e28459528d920b65115c16c04f3efc82aaedc97be59f3f377c0d3f: "00000000000000000000000002e816afc1b5c0f39852131959d946eb3b07b5ad"
      }
  }]
```

### debug_traceBlockFromFile

Similar to [debug_traceBlock](#debug_traceBlock), `traceBlockFromFile` accepts a file containing the RLP of the block.

| Client  | Method invocation                                                                |
|:-------:|----------------------------------------------------------------------------------|
| Go      | `debug.TraceBlockFromFile(fileName string, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlockFromFile(fileName, [options])`                                  |
| RPC     | `{"method": "debug_traceBlockFromFile", "params": [fileName, {}]}`               |

References:
[RLP](https://github.com/ethereum/wiki/wiki/RLP)

### debug_traceBlockByNumber

Similar to [debug_traceBlock](#debug_traceBlock), `traceBlockByNumber` accepts a block number and will replay the
block that is already present in the database.

| Client  | Method invocation                                                              |
|:-------:|--------------------------------------------------------------------------------|
| Go      | `debug.TraceBlockByNumber(number uint64, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlockByNumber(number, [options])`                                  |
| RPC     | `{"method": "debug_traceBlockByNumber", "params": [number, {}]}`               |

References:
[RLP](https://github.com/ethereum/wiki/wiki/RLP)

### debug_traceBlockByHash

Similar to [debug_traceBlock](#debug_traceBlock), `traceBlockByHash` accepts a block hash and will replay the
block that is already present in the database.

| Client  | Method invocation                                                               |
|:-------:|---------------------------------------------------------------------------------|
| Go      | `debug.TraceBlockByHash(hash common.Hash, config. *vm.Config) BlockTraceResult` |
| Console | `debug.traceBlockByHash(hash, [options])`                                       |
| RPC     | `{"method": "debug_traceBlockByHash", "params": [hash {}]}`                     |

References:
[RLP](https://github.com/ethereum/wiki/wiki/RLP)

### debug_dumpBlock

Retrieves the state that corresponds to the block number and returns a list of accounts (including
storage and code).

| Client  | Method invocation                                     |
|:-------:|-------------------------------------------------------|
| Go      | `debug.DumpBlock(number uint64) (state.World, error)` |
| Console | `debug.traceBlockByHash(number, [options])`           |
| RPC     | `{"method": "debug_dumpBlock", "params": [number]}`   |

#### Example

```javascript
> debug.dumpBlock(10)
{
    fff7ac99c8e4feb60c9750054bdc14ce1857f181: {
      balance: "49358640978154672",
      code: "",
      codeHash: "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
      nonce: 2,
      root: "56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
      storage: {}
    },
    fffbca3a38c3c5fcb3adbb8e63c04c3e629aafce: {
      balance: "3460945928",
      code: "",
      codeHash: "c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470",
      nonce: 657,
      root: "56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421",
      storage: {}
    }
  },
  root: "19f4ed94e188dd9c7eb04226bd240fa6b449401a6c656d6d2816a87ccaf206f1"
}
```

### debug_getBlockRlp

Retrieves and returns the RLP encoded block by number.

| Client  | Method invocation                                     |
|:-------:|-------------------------------------------------------|
| Go      | `debug.GetBlockRlp(number uint64) (string, error)`    |
| Console | `debug.getBlockRlp(number, [options])`                |
| RPC     | `{"method": "debug_getBlockRlp", "params": [number]}` |

References: [RLP](https://github.com/ethereum/wiki/wiki/RLP)

### debug_seedHash

Fetches and retrieves the seed hash of the block by number

| Client  | Method invocation                                  |
|:-------:|----------------------------------------------------|
| Go      | `debug.SeedHash(number uint64) (string, error)`    |
| Console | `debug.seedHash(number, [options])`                |
| RPC     | `{"method": "debug_seedHash", "params": [number]}` |

References:
[Ethash](https://github.com/ethereum/wiki/wiki/Mining#the-algorithm)

### debug_setHead

Sets the current head of the local chain by block number. **Note**, this is a
destructive action and may severely damage your chain. Use with *extreme* caution.

| Client  | Method invocation                                 |
|:-------:|---------------------------------------------------|
| Go      | `debug.SetHead(number uint64)`                    |
| Console | `debug.setHead(number)`                           |
| RPC     | `{"method": "debug_setHead", "params": [number]}` |

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

### personal_listAccounts
List all accounts

#### Parameters
none

#### Return
`array` collection with accounts

#### Example
` personal.listAccounts`

***

### personal_newAccount
Create a new account

#### Parameters
`string`, passphrase to protect the account

#### Return
`string` address of the new account

#### Example
` personal.newAccount("mypasswd")`

***

### personal_unlockAccount
Unlock an account

#### Parameters
`string`, address of the account to unlock

`string`, passphrase of the account to unlock (optional in console, user will be prompted if empty)

`integer`, unlock account for duration seconds, use 0 if the account must be locked forever. If not specified 300 is used by default.

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
