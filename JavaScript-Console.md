The `ethereum CLI` executable `geth` has a JavaScript console, which can be started with the `console` subcommand:

    $ geth console

If you need log information, start with:

    $ geth -logfile /tmp/eth.log -loglevel 5 console 2>> /tmp/eth.glog

Otherwise mute your logs, so that it does not pollute your console:

    $ geth -logfile /dev/null console 2>> /dev/null

or 

    $ geth -loglevel 0 console


It's also possible to pass files to the JavaScript intepreter. Load and execute any number of files by giving files as arguments to the `js` subcommand: 

    $ geth js script1.js script2.js

If you want to have the console as well as load scripts, use [loadScript](#loadscript).

## Caveat 

The go-ethereum CLI uses the [Otto JS VM](https://github.com/robertkrimen/otto) which has some limitations:

* "use strict" will parse, but does nothing.
* The regular expression engine (re2/regexp) is not fully compatible with the ECMA5 specification.
* missing `setTimeout` and `setInterval`. These timing functions are not actually part of the ECMA-262 specification. Typically, they belong to the windows object (in the browser).

Since `ethereum.js` uses the [`bignumer.js`](https://github.com/MikeMcl/bignumber.js) library (MIT Expat Licence), it is also autoloded.

## Console API

Ethereum's Javascript console exposes admin functionality and the full [JavaScript API](https://github.com/ethereum/wiki/wiki/JavaScript-API).

* [admin](#admin)
  * [nodeInfo](#adminnodeinfo)
  * [suggestPeer](#adminsuggestpeer)
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
  * [debug](#admindebuggetblockrlp)
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

#### admin
The `admin` exposes the methods to administrate the node.

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

#### admin.suggestPeer

    admin.suggestPeer(nodeURL)

Pass a `nodeURL` to suggest a new peer to the network. The `nodeURL`need to be in a [enode url format](https://github.com/ethereum/wiki/wiki/enode-url-format).

You can find out your own enode by using [nodeInfo](#adminnodeinfo) or looking at the logs when the node boots up e.g.:

```
[P2P Discovery] Listening, enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303
```

*Note*: there is good reason this function is called `suggestPeer` and not `addPeer` (as well as reason why `removePeer` is not available. It is ultimately the p2p server's discretion to connect and disconnect peers depending on number of factors. In normal cases however, when the number of maxpeers is not saturated and your suggested peer is well behaved and compatible, connection may even persist for a long time. 

##### Returns

`true` on success.

##### Example

```javascript
admin.suggestPeer('enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303')
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

Unlock the account for the time `timeout` in seconds. If password is undefined, the user is prompted for it.

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

`true` on success, otherwise `false`.

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

##### Returns

`true` on success, otherwise `false`.

##### Example

```javascript
admin.startRPC("127.0.0.1", 8545, )
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

#### admin.miner.start

    admin.miner.start(threadNumber)

Starts mining on with the given `threadNumber` of parallel threads.

**Note** threadNumber is currently idle until thread safety in ethash is fixed.

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

`String` Returns the current hash rate in KH/s.

***

#### admin.miner.setExtra

    admin.miner.setExtra("extra data")

**Sets** the extra data for the block when finding a block. Limited to 1024 bytes.

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


#### admin.dumpBlock

    admin.dumpBlock(numberOrHash)

##### Returns

the raw dump of a block referred to by block number or block hash or undefined if the block is not found. If the argument is missing or is -1, the current head block is returned.
see [web3.eth.getBlock](https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethgetblock) for more details on block fields and lookup by number or hash.

##### Example


```js
> admin.dumpBlock()
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
The `net` is a shortcut for `web3.net`.

***

#### eth
The `eth` is a shortcut for `web3.eth`.

***

#### shh
The `shh` is a shortcut for `web3.shh`.

***

#### db
The `db` is a shortcut for `web3.db`.

***

