Beside the official [DApp APIs](https://github.com/ethereum/wiki/wiki/JSON-RPC) interface go-ethereum has support for additional management APIs. Similar to the DApp APIs, these are also provided using [JSON-RPC](http://www.jsonrpc.org/specification) and follow exactly the same conventions. Geth comes with a console client which has support for all additional APIs described here.

## Enabling the management APIs

To offer these APIs over the Geth RPC endpoints, please specify them with the `--${interface}api` command line argument (where `${interface}` can be `rpc` for the HTTP endpoint, `ws` for the WebSocket endpoint and `ipc` for the unix socket (Unix) or named pipe (Windows) endpoint).

For example: `geth --ipcapi admin,eth,miner --rpcapi eth,web3 --rpc`

* Enables the admin, official DApp and miner API over the IPC interface
* Enables the official DApp and web3 API over the HTTP interface

The HTTP RPC interface must be explicitly enabled using the `--rpc` flag.

Please note, offering an API over the HTTP (`rpc`) or WebSocket (`ws`) interfaces will give everyone access to the APIs who can access this interface (DApps, browser tabs, etc). Be careful which APIs you enable. By default Geth enables all APIs over the IPC (`ipc`) interface and only the `db`, `eth`, `net` and `web3` APIs over the HTTP and WebSocket interfaces.

To determine which APIs an interface provides, the `modules` JSON-RPC method can be invoked. For example over an `ipc` interface on unix systems:

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

These additional APIs follow the same conventions as the official DApp APIs. Web3 can be [extended](https://github.com/ethereum/web3.js/pull/229) and used to consume these additional APIs. 

The different functions are split into multiple smaller logically grouped APIs. Examples are given for the [JavaScript console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console) but can easily be converted to an RPC request.

**2 examples:**

* Console: `miner.start()`

* IPC: `echo '{"jsonrpc":"2.0","method":"miner_start","params":[],"id":1}' | nc -U $datadir/geth.ipc`

* HTTP: `curl -X POST --data '{"jsonrpc":"2.0","method":"miner_start","params":[],"id":74}' localhost:8545`

With the number of THREADS as an arguments:

* Console: `miner.start(4)`

* IPC: `echo '{"jsonrpc":"2.0","method":"miner_start","params":[4],"id":1}' | nc -U $datadir/geth.ipc`

* HTTP: `curl -X POST --data '{"jsonrpc":"2.0","method":"miner_start","params":[4],"id":74}' localhost:8545`

## List of management APIs

Beside the officially exposed DApp API namespaces (`eth`, `shh`, `web3`), Geth provides the following extra management API namespaces:

* `admin`: Geth node management
* `debug`: Geth node debugging
* `miner`: Miner and [DAG](https://github.com/ethereum/wiki/wiki/Ethash-DAG) management
* `personal`: Account management
* `txpool`: Transaction pool inspection

| admin                             | [debug](#debug)                                 | miner                               | personal                                 | txpool                   |
|:---------------------------------:|:-----------------------------------------------:|:-----------------------------------:|:----------------------------------------:|:------------------------:|
| [addPeer](#admin_addPeer)         | [dumpBlock](#debug_dumpblock)                   | [start](#miner_start)               | [listAccounts](#personal_listAccounts)   | [status](#txpool_status) |
| [peers] (#admin_peers)            | [getBlockRlp] (#debug_getblockrlp)              | [stop](#miner_stop)                 | [newAccount](#personal_newAccount)       |                          |
| [importChain](#admin_importChain) | [printBlock](#debug_printblock)                 | [hashrate](#miner_hashrate)         | [unlockAccount](#personal_unlockAccount) |                          |
| [exportChain](#admin_exportChain) | [processBlock](#debug_processblock)             | [setExtra](#miner_setExtra)         |                                          |                          |
| [verbosity](#admin_verbosity)     | [seedHash](#debug_seedhash)                     | [setGasPrice](#miner_setGasPrice)   |                                          |                          |
| [setSolc](#admin_setColc)         | [setHead](#debug_sethead)                       | [startAutoDAG](#miner_startAutoDAG) |                                          |                          |
| [startRPC](#admin_startRPC)       | [traceTransaction](#debug_tracetrancaction)     | [stopAutoDAG](#miner_stopAutoDAG)   |                                          |                          |
| [stopRPC](#admin_stopRPC)         | [traceBlock](#debug_traceblock)                 | [makeDAG](#miner_makeDAG)           |                                          |                          |
| [startWS](#admin_startWS)         | [traceBlockFromFile](#debug_traceblockfromfile) |                                     |                                          |                          |
| [stopWS](#admin_stopWS)           | [traceBlockByNumber](#debug_blockbynumber)      |                                     |                                          |                          |
|                                   | [getBlockRlp](#debug_getblockrlp)               |                                     |                                          |                          |
|                                   | [seedHash](#debug_seedhash)                     |                                     |                                          |                          |
|                                   | [setHead](#debug_sethead)                       |                                     |                                          |                          |

## API endpoint reference

### admin_addPeer

Requests connecting to a remote node and also maintaining the new connection
at all times, even reconnecting if it is lost.

| Client  | Method invocation                              |
|:-------:|------------------------------------------------|
| Go      | `admin.AddPeer(url string) bool`               |
| Console | `admin.addPeer(url)` returns `boolean`         |
| RPC     | `{"method": "admin_addPeer", "params": [url]}` |

Reference:
 * `url`: [enode](https://github.com/ethereum/wiki/wiki/enode-url-format) URL of the remote peer to start tracking
   * `"enode://4d19a2d...167fa41@XXX.XXX.XXX.XXX:30303"`
 * `return`: indicates whether the peer was accepted for tracking

***

### admin_peers
This property will show all connected peers.

#### Example
`admin.peers`
***

### admin_importChain
Import an exported chain from file into node. This only works if no chain already exists: it does not delete any existing data.

#### Parameters
* `Filename`, the fully qualified path to the file containing the chain to be imported

#### Return
`boolean` indicating if chain was imported

#### Example
`admin.importChain("/tmp/chain.txt")`
***

### admin_exportChain
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

### admin_startRPC
Start the HTTP RPC interface

#### Parameters
(support for optional arguments requires geth version >=1.4)
* `ListenAddress`, open listener on this host (optional, default "localhost")
* `ListenPort`, open listener on this port (optional, default 8545)
* `CorsDomain`, the cross origin resource shared header (optional, default"")
* `Apis`, comma separated list with the API modules which are offered over this interface (optional, default "eth,net,web3")

#### Return
`boolean` indication if the interface was started

#### Example
`admin.startRPC("127.0.0.1", 8545, "*", "eth,net,web3")`

`admin.startRPC(null, null, "http://chriseth.github.io", null)`
***

### admin_stopRPC
Stop the HTTP RPC interface

#### Return
`boolean` indication if the interface was stopped

#### Example
`admin.stopRPC()`

***

### admin_startWS
Start the websocket RPC interface (requires geth version >=1.4)

#### Parameters
* `ListenAddress`, open listener on this host (optional, default "localhost")
* `ListenPort`, open listener on this port (optional, default 8546)
* `allowedDomains`, server side check on the Origin header (optional, default "")
* `Apis`, comma separated list with the API modules which are offered over this interface (optional, default "eth,net,web3")

#### Return
`boolean` indication if the interface was started

#### Example
`admin.startWS("127.0.0.1", 8546, "*", "eth,net,web3")`

`admin.startWS(null, null, "http://chriseth.github.io", null)`
***

### admin_stopWS
Stop the websocket RPC interface

#### Return
`boolean` indication if the interface was stopped

#### Example
`admin.stopWS()`

***

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

References:
[RLP](https://github.com/ethereum/wiki/wiki/RLP)

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

### net_peerCount
The number of connected peers

#### Parameters
none

#### Return
`integer` number of peers

#### Example
` net.peerCount`

***

### net_listening
Indication if this node is currently listening for new peers

#### Parameters
none

#### Return
`boolean` indication if this node accepts new peers

#### Example
` net.listening`

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
