# JavaScript Console

Ethereum implements a **javascript runtime environment** (JSRE) that can be used in either interactive (console) or non-interactive (script) mode.
 
The `ethereum CLI` executable `geth` has a JavaScript console (a **Read, Evaluate & Print Loop** = REPL exposing the the JSRE), which can be started with the `console` subcommand:

    $ geth console

If you need log information, start with:

    $ geth --verbosity 5 console 2>> /tmp/eth.log

Otherwise mute your logs, so that it does not pollute your console:

    $ geth console 2>> /dev/null

or 

    $ geth --verbosity 0 console


It's also possible to pass files to the JavaScript intepreter. Load and execute any number of files by giving files as arguments to the `js` subcommand: 

    $ geth js script1.js script2.js

If you want to have the console as well as load scripts, use [loadScript](#loadscript). Find an example script [here](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#example-script)

## Caveat 

The go-ethereum CLI uses the [Otto JS VM](https://github.com/robertkrimen/otto) which has some limitations:

* "use strict" will parse, but does nothing.
* The regular expression engine (re2/regexp) is not fully compatible with the ECMA5 specification.

Since `ethereum.js` uses the [`bignumer.js`](https://github.com/MikeMcl/bignumber.js) library (MIT Expat Licence), it is also autoloded.

Note that the other known limitation of Otto (namely the lack of timers) is taken care. Ethereum JSRE implements both `setTimeout` and `setInterval`. In addition to this, the console provides `sleep(seconds)` as well as a "blocktime sleep" method `waitForBlock(blockNumber)`. 

Ethereum's Javascript console exposes the full [web3 JavaScript Dapp API](https://github.com/ethereum/wiki/wiki/JavaScript-API) and the [admin API](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#javascript-console-api)
.

# JavaScript Console API

* [eth](#eth)
  * [pendingTransactions](#ethpendingtransactions)
  * [resend](#ethresend)
* [admin](#admin)
  * [verbosity](#adminverbosity)
  * [progress](#adminprogress)
  * [nodeInfo](#adminnodeinfo)
  * [addPeer](#adminaddpeer)
  * [peers](#adminpeers)
  * [unlock](#adminunlock)
  * [newAccount](#adminnewaccount)
  * [import](#adminimport)
  * [export](#adminexport)
  * [startRPC](#adminstartrpc)
  * [stopRPC](#adminstoprpc)
  * [miner](#adminminerstart)
    * [start](#adminminerstart)
    * [stop](#adminminerstop)
    * [hashrate](#adminminerhashrate)
    * [setExtra](#adminminersetextra) 
  * [contractInfo](#admincontractinfo)
    * [start](#admincontractinfostart)
    * [stop](#admincontractinfostop)
    * [get](#admincontractinfoget)
    * [register](#admincontractinforegister)
    * [registerUrl](#admincontractinforegisterurl)
  * [debug](#admindebugbacktrace)
    * [backtrace](#admindebugbacktrace)
    * [setHead](#admindebugsethead)
    * [processBlock](#admindebugprocessblock)
    * [getBlockRlp](#admindebuggetblockrlp)
    * [printBlock](#admindebugprintblock)
    * [dumpBlock](#admindebugdumpblock)
* [loadScript](#loadscript)
* [web3](#web3)
* [net](#net)
* [eth](#eth)
* [shh](#shh)
* [db](#db)

***

#### eth
In addition to the `web3` and `eth` interfaces exposed by [web3.js](https://github.com/ethereum/web3.js) a few additional calls are exposed.

***

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
eth.resend(tx, web3.toWei(10, "szabo")
```

***

#### admin
The `admin` exposes the methods to administrate the node.

***

#### admin.progress

    admin.progress()

Prints info on blockchain synching.

###

***

#### admin.verbosity

    admin.verbosity(level)

**Sets** logger verbosity level to _level_. 1-6: silent, error, warn, info, debug, detail

##### Example

    admin.verbosity(6)

***

#### admin.nodeInfo

    admin.nodeInfo()

##### Returns

information on the node.

##### Example

```
> admin.nodeInfo()
{ Name: 'Ethereum(G)/v0.9.0/darwin/go1.4.1', NodeUrl: 'enode://c32e13952965e5f7ebc85b02a2eb54b09d55f553161c6729695ea34482af933d0a4b035efb5600fc5c3ea9306724a8cbd83845bb8caaabe0b599fc444e36db7e@89.42.0.12:30303', NodeID: '0xc32e13952965e5f7ebc85b02a2eb54b09d55f553161c6729695ea34482af933d0a4b035efb5600fc5c3ea9306724a8cbd83845bb8caaabe0b599fc444e36db7e', IP: '89.42.0.12', DiscPort: 30303, TCPPort: 30303, Td: '0', ListenAddr: '[89.42.0.12:30303' }
```

To connect to a node, use the [enode-format](https://github.com/ethereum/wiki/wiki/enode-url-format) nodeUrl as an argument to [suggestPeer](#adminsuggestpeer) or with CLI param `bootnodes`.

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
admin.addPeer('enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303')
```

***

#### admin.peers

    admin.peers()

##### Returns

an array of objects with information about connected peers.

##### Example

```
> admin.peers()
[ { ID: '0x6cdd090303f394a1cac34ecc9f7cda18127eafa2a3a06de39f6d920b0e583e062a7362097c7c65ee490a758b442acd5c80c6fce4b148c6a391e946b45131365b', Name: 'Ethereum(G)/v0.9.0/linux/go1.4.1', Caps: 'eth/56, shh/2', RemoteAddress: '54.169.166.226:30303', LocalAddress: '10.1.4.216:58888' } { ID: '0x4f06e802d994aaea9b9623308729cf7e4da61090ffb3615bc7124c5abbf46694c4334e304be4314392fafcee46779e506c6e00f2d31371498db35d28adf85f35', Name: 'Mist/v0.9.0/linux/go1.4.2', Caps: 'eth/58, shh/2', RemoteAddress: '37.142.103.9:30303', LocalAddress: '10.1.4.216:62393' } ]
```
***

#### admin.unlock

    admin.unlock(address, password, timeout)

Unlock the account for the time `timeout` in seconds. If password is undefined, the user is prompted for it. If timeout isn't specified a default of 300 seconds is used.

##### Returns

`true` on success, otherwise `false`.

##### Example

```js
> admin.unlock("8dbad2d2b85bbb727ee35b31e64f5092ff54a93e")
Please enter a passphrase now.
Passphrase:
true
>
```

***

#### admin.newAccount

    admin.newAccount(password)

Creates a new account and encrypts it with `password`. If no `password` is given the user is prompted for it. 

##### Returns

account address on success, otherwise `false`.

##### Example

```js
> admin.newAccount()
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
'cf68505ae04bb425eb431eceb629c351bd9a4eee'
>
```
2
***

#### admin.import

    admin.import()

Imports the blockchain from a marshalled binary format.
**Note** that the blockchain is reset (to genesis) before the imported blocks are inserted to the chain.


##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.import('path/to/file')
// true
```

***

#### admin.export

    admin.export()

Exports the blockchain to the given file in binary format.

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.export()
// binary ?
```

***

#### admin.startRPC

     admin.startRPC(ipAddress, portNumber, corsheader)

Starts the HTTP server for the [JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC).
If corsheader is missing or the empty string, CORS header is not set.

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.startRPC("127.0.0.1", 8545, "*")
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
admin.stopRpc()
// true
```

***

### Mining



#### admin.miner.start

    admin.miner.start(threadNumber)

Starts [mining](see https://github.com/ethereum/go-ethereum/wiki/Mining) on with the given `threadNumber` of parallel threads.

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.miner.start(4)
// true
```

***

#### admin.miner.stop

    admin.miner.stop()

Stops all miners.

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.miner.stop()
// true
```

***

#### admin.miner.hashrate
    
    admin.miner.hashrate()

##### Returns

`String` Returns the current hash rate in H/s.

***

#### admin.miner.setExtra

    admin.miner.setExtra("extra data")

**Sets** the extra data for the block when finding a block. Limited to 1024 bytes.

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

#### admin.contractInfo.register

    admin.contractInfo.register(address, contractaddress, contract, filename);

will extract [contract info json](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions#contract-info-metadata) doc from the contract, calculates its hash and registers that content hash to the codehash of the contract on contractaddress. The register transaction is sent from the address in the first parameter.

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
contract = eth.compile.solidity(source);
contractaddress = eth.sendTransaction({from: primary, data: contract.code, gas: "1000000", gasPrice: "100000" });
filename = "/tmp/info.json";
admin.contractInfo.register(primary, contractaddress, contract, filename);
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
contract = eth.compile.solidity(source);
contractaddress = eth.sendTransaction({from: primary, data: contract.code, gas: "1000000", gasPrice: "100000" });
filename = "/tmp/info.json";
contenthash = admin.contractInfo.register(primary, contractaddress, contract, filename);
admin.contractInfo.registerUrl(primary, contenthash, "file://"+filename);
```

***


### Debug

#### admin.debug.backtrace

    admin.debug.backtrace("matcher")

a stack trace will be written to the log whenever execution hits the statement matched by the matcher.
Matcher is file and line number holding a logging statement, e.g., `chain_manager.go:234`

##### Example

    admin.debug.backtrace("chain_manager.go:234")

***

#### admin.debug.setHead

    admin.debug.setHead(hashHexStringOrBlockNumber)

**Sets** the current head of the blockchain to the block referred to by _hashHexStringOrBlockNumber_.
See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Returns 

`true` on success, otherwise `false`.

##### Example 

    admin.debug.setHead(140101)

***

#### admin.debug.processBlock

    admin.debug.processBlock(hashHexStringOrBlockNumber)

Processes the given block referred to by _hashHexStringOrBlockNumber_ with the VM in debug mode.
See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.
In combination with `setHead`, this can be used to replay processing of a block to debug VM execution.

##### Returns 

`true` on success, otherwise `false`.

##### Example 

    admin.debug.processBlock(140101)

***

#### admin.debug.getBlockRlp

    admin.debug.getBlockRlp(hashHexStringOrBlockNumber)

Returns the hexadecimal representation of the RLP encoding of the block.
See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Returns 

The hex representation of the RLP encoding of the block.

##### Example

```
> admin.debug.getBlockRlp(131805)    'f90210f9020ba0ea4dcb53fe575e23742aa30266722a15429b7ba3d33ba8c87012881d7a77e81ea01dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d4934794a4d8e9cae4d04b093aac82e6cd355b6b963fb7ffa01f892bfd6f8fb2ec69f30c8799e371c24ebc5a9d55558640de1fb7ca8787d26da056e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421a056e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421b901000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000083bb9266830202dd832fefd880845534406d91ce9e5448ce9ed0af535048ce9ed0afce9ea04cf6d2c4022dfab72af44e9a58d7ac9f7238ffce31d4da72ed6ec9eda60e1850883f9e9ce6a261381cc0c0'
```

***

#### admin.debug.printBlock

    admin.debug.printBlock(hashHexStringOrBlockNumber)

Prints information about the block such as size, total difficulty, as well as header fields properly formatted.

See [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Returns

`true` on success, otherwise `false`.

##### Example

```
> admin.debug.printBlock(131805)
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


#### admin.debug.dumpBlock

    admin.debug.dumpBlock(numberOrHash)

##### Returns

the raw dump of a block referred to by block number or block hash or undefined if the block is not found. If the argument is missing or is -1, the current head block is returned.
see [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Example


```js
> admin.debug.dumpBlock()
>
```


***


#### loadScript

     loadScript('/path/to/myfile.js');

Loads a JavaScript file and executes it. Relative paths are interpreted as relative to `jspath` which is specified as a command line flag, see [Command Line Options](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options).

***

#### web3
The `web3` exposes all methods of the [JavaScript API](https://github.com/ethereum/wiki/wiki/JavaScript-API).

***

#### net
The `net` is a shortcut for [web3.net](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3net).

***

#### eth
The `eth` is a shortcut for [web3.eth](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3eth).

***

#### shh
The `shh` is a shortcut for [web3.shh](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3shh).

***

#### db
The `db` is a shortcut for [web3.db](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3db).

***