# JavaScript Runtime Environment

Ethereum implements a **javascript runtime environment** (JSRE) that can be used in either interactive (console) or non-interactive (script) mode.
 
Ethereum's Javascript console exposes the full [web3 JavaScript Dapp API](https://github.com/ethereum/wiki/wiki/JavaScript-API) and the [admin API](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#javascript-console-api).

## Interactive use: the JSRE REPL  Console

The `ethereum CLI` executable `geth` has a JavaScript console (a **Read, Evaluate & Print Loop** = REPL exposing the JSRE), which can be started with the `console` or `attach` subcommand. The `console` subcommands starts the geth node and then opens the console. The `attach` subcommand will not start the geth node but instead tries to open the console on a running geth instance.

    $ geth console
    $ geth attach

The attach node accepts an endpoint in case the geth node is running with a non default ipc endpoint or you would like to connect over the rpc interface.

    $ geth attach ipc:/some/custom/path
    $ geth attach rpc:http://191.168.1.1:8545

Note that by default the geth node doesn't start the rpc service and not all functionality is provided over this interface due to security reasons. These defaults can be overridden when the `--rpcapi` argument when the geth node is started, or with [admin.startRPC](admin_startRPC).

If you need log information, start with:

    $ geth --verbosity 5 console 2>> /tmp/eth.log

Otherwise mute your logs, so that it does not pollute your console:

    $ geth console 2>> /dev/null

or 

    $ geth --verbosity 0 console

Note: Since the database can only be accessed by one process, this means you cannot run `geth console` if you have an instance of geth already running.

## Non-interactive use: JSRE script mode

It's also possible to execute files to the JavaScript intepreter. The `console` and `attach` subcommand accept the `--exec` argument which is a javascript statement. 

    $ geth --exec "eth.blockNumber" attach

This prints the current block number of a running geth instance.

Or execute a script with more complex statements with:

    $ geth --exec 'loadScript("/tmp/checkbalances.js")' attach
    $ geth --jspath "/tmp" --exec 'loadScript("checkbalances.js")' attach

Find an example script [here](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#example-script)

Use the `--jspath <path/to/my/js/root>` to set a libdir for your js scripts. Parameters to `loadScript()` with no absolute path will be understood relative to this directory.

You can exit the console cleanly by typing `exit` or simply with `CTRL-C`.

## Caveat 

The go-ethereum JSRE uses the [Otto JS VM](https://github.com/robertkrimen/otto) which has some limitations:

* "use strict" will parse, but does nothing.
* The regular expression engine (re2/regexp) is not fully compatible with the ECMA5 specification.

Note that the other known limitation of Otto (namely the lack of timers) is taken care. Ethereum JSRE implements both `setTimeout` and `setInterval`. In addition to this, the console provides `sleep(seconds)` as well as a "blocktime sleep" method `waitForBlock(blockNumber)`. 

Since `ethereum.js` uses the [`bignumer.js`](https://github.com/MikeMcl/bignumber.js) library (MIT Expat Licence), it is also autoloded.

# Timers

In addition to the full functionality of JS (as per ECMA5), the ethereum JSRE is augmented with various timers. It implements `setInterval`, `clearInterval`, `setTimeout`, `clearTimeout` you may be used to using in browser windows. It also provides implementation for `sleep(seconds)` and a blockchain-height based timer, `sleepBlocks(n)` which sleeps till the number of new blocks added is equal to or greater than `sleepBlocks(n)`, think "wait for n confirmations". 

# JavaScript Console API

* [eth](#eth)
  * [pendingTransactions](#ethpendingtransactions)
  * [resend](#ethresend)
* [admin](#admin)
  * [addPeer](#adminaddpeer)
  * [peers](#adminpeers)  
  * [nodeInfo](#adminnodeinfo)
  * [datadir](#admindatadir)
  * [importChain](#adminimportchain)
  * [exportChain](#adminexportchain)
  * [chainSyncStatus](#adminchainsyncstatus)
  * [startRPC](#adminstartrpc)
  * [stopRPC](#adminstoprpc)
  * [verbosity](#adminverbosity)
  * [setSolc](#adminsetsolc)
  * [sleepBlocks](#adminsleepblocks)
* [miner](#miner)
  * [start](#minerstart)
  * [stop](#minerstop)
  * [startAutoDAG](#minerstartautodag)
  * [stopAutoDAG](#minerstopautodag)
  * [makeDAG](#minermakedag)
  * [hashrate](#minerhashrate)
  * [setExtra](#minersetextra) 
  * [setGasPrice] (#minersetgasprice)
* [personal](#personal)
  * [newAccount](#personalnewaccount)
  * [listAccounts](#personallistaccounts)
  * [deleteAccount](#personaldeleteaccount)
  * [unlockAccount](#personalunlockaccount)
* [contractInfo](#admincontractinfo)
  * [start](#admincontractinfostart)
  * [stop](#admincontractinfostop)
  * [get](#admincontractinfoget)
  * [register](#admincontractinforegister)
  * [registerUrl](#admincontractinforegisterurl)
* [txpool](#txpool)
  * [status](#txpoolstatus)
* [debug](#debug)
  * [setHead](#debugsethead)
  * [seedHash](#debugseedhash)
  * [processBlock](#debugprocessblock)
  * [getBlockRlp](#debuggetblockrlp)
  * [printBlock](#debugprintblock)
  * [dumpBlock](#debugdumpblock)
  * [metrics](#debugmetrics)
* [loadScript](#loadscript)
* [sleep](#sleep)
* [setInterval](#setInterval)
* [clearInterval](#clearInterval)
* [setTimeout](#setTimeout)
* [clearTimeout](#clearTimeout)
* [web3](#web3)
* [net](#net)
* [eth](#eth)
* [shh](#shh)
* [db](#db)

***

#### Personal
The `personal` api exposes method for personal  the methods to manage, control or monitor your node. It allows for limited file system access. 

***

#### personal.listAccounts
    personal.listAccount

List all accounts

#### Return
collection with accounts

#### Example
` personal.listAccounts`

***

#### personal.newAccount

    personal.newAccount(passwd)

Create a new password protected account

#### Return
`string` address of the new account

#### Example
` personal.newAccount("mypasswd")`

***

#### personal.deleteAccount
    personal.deleteAccount(addr, passwd)

Delete the account with the given address and password

#### Return
indication if the account was deleted

#### Example
` personal.deleteAccount(eth.coinbase, "mypasswd")`

***

#### personal.unlockAccount
    personal.unlockAccount(addr, passwd, duration)

Unlock the account with the given address, password and an optional duration (in seconds)

#### Return
`boolean` indication if the account was unlocked

#### Example
` personal.unlockAccount(eth.coinbase, "mypasswd", 300)`

***

### TxPool

#### txpool.status
    txpool.status

Number of pending/queued transactions

#### Return
`pending` all processable transactions

`queued` all non-processable transactions

#### Example
` txpool.status`

***


#### admin
The `admin` exposes the methods to manage, control or monitor your node. It allows for limited file system access. 

***

#### admin.chainSyncStatus

    admin.chainSyncStatus

Prints info on blockchain synching.

### return
`blocksAvailable`, blocks which have not been downloaded

`blocksWaitingForImport`, downloaded blocks waiting before import

`estimate`, a (very rough) estimate before the node has imported all blocks

`importing`, blocks currently importing

***

#### admin.verbosity

    admin.verbosity(level)

**Sets** logger verbosity level to _level_. 1-6: silent, error, warn, info, debug, detail

##### Example

    > admin.verbosity(6)

***

#### admin.nodeInfo

    admin.nodeInfo

##### Returns

information on the node.

##### Example

```
> admin.nodeInfo
{
   Name: 'Ethereum(G)/v0.9.36/darwin/go1.4.1',
   NodeUrl: 'enode://c32e13952965e5f7ebc85b02a2eb54b09d55f553161c6729695ea34482af933d0a4b035efb5600fc5c3ea9306724a8cbd83845bb8caaabe0b599fc444e36db7e@89.42.0.12:30303',
   NodeID: '0xc32e13952965e5f7ebc85b02a2eb54b09d55f553161c6729695ea34482af933d0a4b035efb5600fc5c3ea9306724a8cbd83845bb8caaabe0b599fc444e36db7e',
   IP: '89.42.0.12',
   DiscPort: 30303,
   TCPPort: 30303,
   Td: '0',
   ListenAddr: '[::]:30303'
}
```

To connect to a node, use the [enode-format](https://github.com/ethereum/wiki/wiki/enode-url-format) nodeUrl as an argument to [addPeer](#adminaddpeer) or with CLI param `bootnodes`.

***

#### admin.addPeer

    admin.addPeer(nodeURL)

Pass a `nodeURL` to connect a to a peer on the network. The `nodeURL` needs to be in [enode URL format](https://github.com/ethereum/wiki/wiki/enode-url-format). geth will maintain the connection until it
shuts down and attempt to reconnect if the connection drops intermittently.

You can find out your own node URL by using [nodeInfo](#adminnodeinfo) or looking at the logs when the node boots up e.g.:

```
[P2P Discovery] Listening, enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303
```

##### Returns

`true` on success.

##### Example

```javascript
> admin.addPeer('enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303')
```

***

#### admin.peers

    admin.peers

##### Returns

an array of objects with information about connected peers.

##### Example

```
> admin.peers
[ { ID: '0x6cdd090303f394a1cac34ecc9f7cda18127eafa2a3a06de39f6d920b0e583e062a7362097c7c65ee490a758b442acd5c80c6fce4b148c6a391e946b45131365b', Name: 'Ethereum(G)/v0.9.0/linux/go1.4.1', Caps: 'eth/56, shh/2', RemoteAddress: '54.169.166.226:30303', LocalAddress: '10.1.4.216:58888' } { ID: '0x4f06e802d994aaea9b9623308729cf7e4da61090ffb3615bc7124c5abbf46694c4334e304be4314392fafcee46779e506c6e00f2d31371498db35d28adf85f35', Name: 'Mist/v0.9.0/linux/go1.4.2', Caps: 'eth/58, shh/2', RemoteAddress: '37.142.103.9:30303', LocalAddress: '10.1.4.216:62393' } ]
```
***

#### admin.importChain

    admin.importChain()

Imports the blockchain from a marshalled binary format.
**Note** that the blockchain is reset (to genesis) before the imported blocks are inserted to the chain.


##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.importChain('path/to/file')
// true
```

***

#### admin.exportChain

    admin.exportChain()

Exports the blockchain to the given file in binary format.

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.exportChain('path/to/file')
```

***

#### admin.startRPC

     admin.startRPC(ipAddress, portNumber, corsheader, modules)

Starts the HTTP server for the [JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC). 

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.startRPC("127.0.0.1", 8545, "*", "web3,db,net,eth")
// true
```

***

#### admin.stopRPC

    admin.stopRPC() 

Stops the HTTP server for the [JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC).

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.stopRPC()
// true
```

***

#### admin.sleepBlocks 

    admin.sleepBlocks(n)

Sleeps for n blocks. 

***

#### admin.datadir

    admin.datadir

the directory this nodes stores its data

##### Returns

directory on success

##### Example

```javascript
admin.datadir
'/Users/username/Library/Ethereum'
```

***

#### admin.setSolc

    admin.setSolc()

Set the solidity compiler

##### Returns

a string describing the compiler version when path was valid, otherwise an error

##### Example

```javascript
admin.setSolc('/some/path/solc')
'solc v0.9.29
Solidity Compiler: /some/path/solc
'
```

***

### Miner



#### miner.start

    miner.start(threadCount)

Starts [mining](see https://github.com/ethereum/go-ethereum/wiki/Mining) on with the given `threadNumber` of parallel threads. This is an optional argument.

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
miner.start()
// true
```

***

#### miner.stop

    miner.stop(threadCount)

Stops `threadCount` miners. This is an optional argument.

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
miner.stop()
// true
```

***


#### miner.startAutoDAG

    miner.startAutoDAG()

Starts automatic pregeneration of the [ethash DAG](https://github.com/ethereum/wiki/wiki/Ethash-DAG). This process make sure that the DAG for the subsequent epoch is available allowing mining right after the new epoch starts. If this is used by most network nodes, then blocktimes are expected to be normal at epoch transition. Auto DAG is switched on automatically when mining is started and switched off when the miner stops. 

##### Returns

`true` on success, otherwise `false`.

***

#### miner.stopAutoDAG

    miner.stopAutoDAG()

Stops automatic pregeneration of the [ethash DAG](https://github.com/ethereum/wiki/wiki/Ethash-DAG). Auto DAG is switched off automatically when mining is stops.

##### Returns

`true` on success, otherwise `false`.

***

#### miner.makeDAG

    miner.makeDAG(blockNumber, dir)

Generates the DAG for epoch `blockNumber/epochLength`. dir specifies a target directory,
If `dir` is the empty string, then ethash will use the default directories `~/.ethash` on Linux and MacOS, and `~\AppData\Ethash` on Windows. The DAG file's name is `full-<revision-number>R-<seedhash>`

##### Returns

`true` on success, otherwise `false`.

***

#### miner.hashrate
    
    miner.hashrate

##### Returns

Returns the current hash rate in H/s.

***

#### miner.setExtra

    miner.setExtra("extra data")

**Sets** the extra data for the block when finding a block. Limited to 1024 bytes.

***

#### miner.setGasPrice

    miner.setGasPrice()

**Sets** the the gasprice for the miner

***


### Contract Info

#### admin.contractInfo.start

     admin.contractInfo.start()

activate NatSpec: when sending a transaction to a contract, 
Registry lookup and url fetching is used to retrieve authentic contract Info for it. It allows for prompting a user with authentic contract-specific confirmation messages.

***

#### admin.contractInfo.stop

     admin.contractInfo.stop()

deactivate NatSpec: when sending a transaction, the user  will be prompted with a generic confirmation message, no contract info is fetched

***

#### admin.contractInfo.get

     admin.contractInfo.get(address)

this will retrieve the [contract info json](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#contract-info-metadata) for a contract on the address

##### Returns

returns the contract info object 

##### Examples

```js
> info = admin.contractInfo.get(contractaddress)
> source = info.source
> abi = info.abiDefinition
```

***
#### admin.contractInfo.saveInfo

    admin.contractInfo.saveInfo(contract.info, filename);

will write [contract info json](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#contract-info-metadata) into the target file, calculates its content hash. This content hash then can used to associate a public url with where the contract info is publicly available and verifiable. If you register the codehash (hash of the code of the contract on contractaddress).

##### Returns

`contenthash` on success, otherwise `undefined`.

##### Examples

```js
source = "contract test {\n" +
"   /// @notice will multiply `a` by 7.\n" +
"   function multiply(uint a) returns(uint d) {\n" +
"      return a * 7;\n" +
"   }\n" +
"} ";
contract = eth.compile.solidity(source).test;
contractaddress = eth.sendTransaction({from: primary, data: contract.code });
filename = "/tmp/info.json";
contenthash = admin.contractInfo.saveInfo(contract.info, filename);
```

***
#### admin.contractInfo.register

    admin.contractInfo.register(address, contractaddress, contenthash);

will register content hash to the codehash (hash of the code of the contract on contractaddress). The register transaction is sent from the address in the first parameter. The transaction needs to be processed and confirmed on the canonical chain for the registration to take effect.

##### Returns

`true` on success, otherwise `false`.

##### Examples

```js
source = "contract test {\n" +
"   /// @notice will multiply `a` by 7.\n" +
"   function multiply(uint a) returns(uint d) {\n" +
"      return a * 7;\n" +
"   }\n" +
"} ";
contract = eth.compile.solidity(source).test;
contractaddress = eth.sendTransaction({from: primary, data: contract.code });
filename = "/tmp/info.json";
contenthash = admin.contractInfo.saveInfo(contract.info, filename);
admin.contractInfo.register(primary, contractaddress, contenthash);
```

***

#### admin.contractInfo.registerUrl

    admin.contractInfo.registerUrl(address, codehash, contenthash);

this will register a contant hash to the contract' codehash. This will be used to locate [contract info json](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#contract-info-metadata)
files. Address in the first parameter will be used to send the transaction. 

##### Returns

`true` on success, otherwise `false`.

##### Examples

```js
source = "contract test {\n" +
"   /// @notice will multiply `a` by 7.\n" +
"   function multiply(uint a) returns(uint d) {\n" +
"      return a * 7;\n" +
"   }\n" +
"} ";
contract = eth.compile.solidity(source).test;
contractaddress = eth.sendTransaction({from: primary, data: contract.code, gas: "1000000", gasPrice: "100000" });
filename = "/tmp/info.json";
contenthash = admin.contractInfo.register(primary, contractaddress, contract, filename);
admin.contractInfo.registerUrl(primary, contenthash, "file://"+filename);
```

***


### Debug

#### debug.setHead

    debug.setHead(hashHexStringOrBlockNumber)

**Sets** the current head of the blockchain to the block referred to by _hashHexStringOrBlockNumber_.
See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Returns 

`true` on success, otherwise `false`.

##### Example 

    debug.setHead(eth.blockNumber-1000)

***

#### debug.seedHash

    debug.seedHash(hashHexStringOrBlockNumber)

Returns the hash for the epoch the given block is in.

##### Returns 

hash in hex format

##### Example 

    > debug.seedHash(eth.blockNumber)
    '0xf2e59013a0a379837166b59f871b20a8a0d101d1c355ea85d35329360e69c000'

***

#### debug.processBlock

    debug.processBlock(hashHexStringOrBlockNumber)

Processes the given block referred to by _hashHexStringOrBlockNumber_ with the VM in debug mode.
See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.
In combination with `setHead`, this can be used to replay processing of a block to debug VM execution.

##### Returns 

`true` on success, otherwise `false`.

##### Example 

    debug.processBlock(140101)

***


#### debug.getBlockRlp

    debug.getBlockRlp(hashHexStringOrBlockNumber)

Returns the hexadecimal representation of the RLP encoding of the block.
See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Returns 

The hex representation of the RLP encoding of the block.

##### Example

```
> debug.getBlockRlp(131805)    'f90210f9020ba0ea4dcb53fe575e23742aa30266722a15429b7ba3d33ba8c87012881d7a77e81ea01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d4934794a4d8e9cae4d04b093aac82e6cd355b6b963fb7ffa01f892bfd6f8fb2ec69f30c8799e371c24ebc5a9d55558640de1fb7ca8787d26da056e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421a056e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421b901000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000083bb9266830202dd832fefd880845534406d91ce9e5448ce9ed0af535048ce9ed0afce9ea04cf6d2c4022dfab72af44e9a58d7ac9f7238ffce31d4da72ed6ec9eda60e1850883f9e9ce6a261381cc0c0'
```

***

#### debug.printBlock

    debug.printBlock(hashHexStringOrBlockNumber)

Prints information about the block such as size, total difficulty, as well as header fields properly formatted.

See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Returns

formatted string representation of the block

##### Example

```
> debug.printBlock(131805)
BLOCK(be465b020fdbedc4063756f0912b5a89bbb4735bd1d1df84363e05ade0195cb1): Size: 531.00 B TD: 643485290485 {
NoNonce: ee48752c3a0bfe3d85339451a5f3f411c21c8170353e450985e1faab0a9ac4cc
Header:
[

        ParentHash:         ea4dcb53fe575e23742aa30266722a15429b7ba3d33ba8c87012881d7a77e81e
        UncleHash:          1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347
        Coinbase:           a4d8e9cae4d04b093aac82e6cd355b6b963fb7ff
        Root:               1f892bfd6f8fb2ec69f30c8799e371c24ebc5a9d55558640de1fb7ca8787d26d
        TxSha               56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421
        ReceiptSha:         56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421
        Bloom:              00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
        Difficulty:         12292710
        Number:             131805
        GasLimit:           3141592
        GasUsed:            0
        Time:               1429487725
        Extra:              ΞTHΞЯSPHΞЯΞ
        MixDigest:          4cf6d2c4022dfab72af44e9a58d7ac9f7238ffce31d4da72ed6ec9eda60e1850
        Nonce:              3f9e9ce6a261381c
]
Transactions:
[]
Uncles:
[]
}


```


***


#### debug.dumpBlock

    debug.dumpBlock(numberOrHash)

##### Returns

the raw dump of a block referred to by block number or block hash or undefined if the block is not found. 
see [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Example


```js
> debug.dumpBlock(eth.blockNumber)
```


***

#### debug.metrics

    debug.metrics(raw)

##### Returns

Collection metrics

##### Example


```js
> metrics(true)
```


***


#### loadScript

     loadScript('/path/to/myfile.js');

Loads a JavaScript file and executes it. Relative paths are interpreted as relative to `jspath` which is specified as a command line flag, see [Command Line Options](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options). 

#### sleep 

    sleep(s)

Sleeps for s seconds. 

#### setInterval

    setInterval(s, func() {})

#### clearInterval
#### setTimeout
#### clearTimeout


***

#### web3
The `web3` exposes all methods of the [JavaScript API](https://github.com/ethereum/wiki/wiki/JavaScript-API).

***

#### net
The `net` is a shortcut for [web3.net](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3net).

***

#### eth
The `eth` is a shortcut for [web3.eth](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3eth). In addition to the `web3` and `eth` interfaces exposed by [web3.js](https://github.com/ethereum/web3.js) a few additional calls are exposed.

***

#### eth.sign

    eth.sign(signer, data)

#### eth.pendingTransactions

    eth.pendingTransactions()

Returns pending transactions that belong to one of the users `eth.accounts`.

***

#### eth.resend

    eth.resend(tx, <optional gas price>, <optional gas limit>)

Resends the given transaction returned by `pendingTransactions()` and allows you to overwrite the gas price and gas limit of the transaction.

##### Example

```javascript
eth.sendTransaction({from: eth.accounts[0], to: "...", gasPrice: "1000"})
var tx = eth.pendingTransactions()[0]
eth.resend(tx, web3.toWei(10, "szabo"))
```

***


#### shh
The `shh` is a shortcut for [web3.shh](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3shh).

***

#### db
The `db` is a shortcut for [web3.db](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3db).

***