The `ethereum CLI` executable has a JavaScript console, which can be started with the `console` subcommand:

    $ ethereum console

If you need log information, start with:

    $ ethereum -logfile /tmp/eth.log -loglevel 5 console

It's also possible to pass files to the JavaScript intepreter. Load and execute any number of files by giving files as arguments to the `console` subcommand: 

    $ ethereum console script1.js script2.js

## Caveat 

The go-ethereum CLI uses the [Otto JS VM](https://github.com/obscuren/otto) (forked from https://github.com/robertkrimen/otto) which has some limitations:

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
  * [startRpc](#adminstartrpc)
  * [stopRpc](#adminstoprpc)
  * [startMining](#adminstartmining)
  * [stopMining](#adminstopmining)
  * [unlock](#adminunlock)
  * [newAccount](#adminnewaccount)
  * [dumpBlock](#admindumpblock)
  * [import](#adminimport)
  * [export](#adminexport)
* [loadScript](#loadscript)
* [web3](#web3)
* [net](#net)
* [eth](#eth)
* [shh](#shh)
* [db](#db)

***

##### admin
The `admin` exposes the methods to administrate the node.

***

##### admin.nodeInfo

    admin.nodeInfo()

**Returns** information on the node.

```javascript
admin.nodeInfo()
// {...}
```

```
> admin.nodeInfo()
{ Name: 'Ethereum(G)/v0.9.0/darwin/go1.4.1', NodeUrl: 'enode://c32e13952965e5f7ebc85b02a2eb54b09d55f553161c6729695ea34482af933d0a4b035efb5600fc5c3ea9306724a8cbd83845bb8caaabe0b599fc444e36db7e@89.42.0.12:30303', NodeID: '0xc32e13952965e5f7ebc85b02a2eb54b09d55f553161c6729695ea34482af933d0a4b035efb5600fc5c3ea9306724a8cbd83845bb8caaabe0b599fc444e36db7e', IP: '89.42.0.12', DiscPort: 30303, TCPPort: 30303, Td: '0', ListenAddr: '[89.42.0.12:30303' }
```

***

##### admin.suggestPeer

    admin.suggestPeer(nodeURL)

Pass a `nodeURL` to suggest a new peer to the network. The `nodeURL`need to be in a [enode url format](https://github.com/ethereum/wiki/wiki/enode-url-format).

You can find out your own enode by looking at the logs when the node boots up e.g.:

```
[P2P Discovery] Listening, enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303
```

**Returns** `true` on success.

```javascript
admin.suggestPeer('enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303')
```

***

##### admin.peers

    admin.peers()

**Returns** an array of objects with information about connected peers.

```javascript
admin.peers()
// [{...},{...}]
```

```
> admin.peers()
[ { ID: '0x6cdd090303f394a1cac34ecc9f7cda18127eafa2a3a06de39f6d920b0e583e062a7362097c7c65ee490a758b442acd5c80c6fce4b148c6a391e946b45131365b', Name: 'Ethereum(G)/v0.9.0/linux/go1.4.1', Caps: 'eth/56, shh/2', RemoteAddress: '54.169.166.226:30303', LocalAddress: '10.1.4.216:58888' } { ID: '0x4f06e802d994aaea9b9623308729cf7e4da61090ffb3615bc7124c5abbf46694c4334e304be4314392fafcee46779e506c6e00f2d31371498db35d28adf85f35', Name: 'Mist/v0.9.0/linux/go1.4.2', Caps: 'eth/58, shh/2', RemoteAddress: '37.142.103.9:30303', LocalAddress: '10.1.4.216:62393' } ]
```
***

##### admin.startRpc

    admin.startRpc(portNumber)

Starts the HTTP server for the [JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC).

**Returns** `true` on success, otherwise `false`.

```javascript
admin.startRpc(8545)
// true
```

***

##### admin.stopRpc

    admin.stopRpc() [not implemented]

Stops the HTTP server for the [JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC).

**Returns** `true` on success, otherwise `false`.

```javascript
admin.stopRpc()
// true
```

***

##### admin.startMining

    admin.startMining(threadNumber)

Starts mining on with the given `threadNumber` of parallel threads.

**Returns** `true` on success, otherwise `false`.

```javascript
admin.startMining(4)
// true
```

***

##### admin.stopMining

    admin.stopMining()

Stops all miners.

**Returns** `true` on success, otherwise `false`.

```javascript
admin.stopMining()
// true
```

***

##### admin.unlock

    admin.unlock(address, password, timeout)

Unlock the account for the time `timeout` in seconds. If password is undefined, the user is prompted for it.

**Returns** `true` on success, otherwise `false`.

```javascript
admin.unlock('0x833b8ed5a2957e5b050ba6539efa66cb67165eec', '1234', 1000 * 60 * 10) // unlock for 10 mins
// true
```

```
> admin.unlock("8dbad2d2b85bbb727ee35b31e64f5092ff54a93e")
Please enter a passphrase now.
Passphrase:
true
>
```

***

##### admin.newAccount

    admin.newAccount(password)

Creates a new account and encrypts it with `password`. If no `password` is given the user is prompted for it. 

**Returns** `true` on success, otherwise `false`.

```javascript
admin.newAccount('1234')
// true
```

```
> admin.newAccount()
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
'cf68505ae04bb425eb431eceb629c351bd9a4eee'
>
```
***

* `dumpBlock(numberOrHash)`
returns the raw dump of a block referred to by block number or block hash or undefined if the block is not found.

##### admin.dumpBlock

    admin.dumpBlock(numberOrHash)

**Returns** the raw dump of a block referred to by block number or block hash or undefined if the block is not found. If the argument is missing or is -1, the current head block is returned.

```javascript
admin.dumpBlock(29)
// {...}
```

```
> admin.dumpBlock()
{ Root: 'e26e11f5252fca8b61d5979c0dea584c22d0e1b3bf687c915276c90c04bdb4bb', Accounts: { 2091fc869cbf49e84e72365bfbaee012f7f0c3ad368ef8c56c167ba75538c1ad: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, 5b70e80538acdabd6137353b0f9d8d149f4dba91e8be2e7946e409bfdbe685b9: { Balance: '1', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, 9bc17373cefddb760ffc916a98ea7152ceb68a400a74fb36e770b695e16e48b0: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, a28629e41841cc5e702899227a10d8fd7debed9ce87f47a95dcc3bedd9a8a4ef: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, cc7fbca600b4bbd1d889076147f61721f0f04cef8b3f1a6ee8097f454543ce55: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, d52688a8f926c816ca1e079067caba944f158e764817b83fc43594370ca9cf62: { Balance: '1', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, e044041bfd3b93fab85f2a1f96c7993754ae741bfe6fee5992f8b23dd181992c: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, 1468288056310c82aa4c01a7e12a10f8111a0560e72b700555479031b86c357d: { Balance: '1', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, 70009f4500e4fc823b89deed6bb8bfb9b4d3edcffb92a431d65d6e3b920943d7: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, 8782e9da2bab05079eceaaa3551f40e16fb8f0c47476b00498a13b25e1de8dd6: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, a876da518a393dbd067dc72abfa08d475ed6447fca96d92ec3f9e7eba503ca61: { Balance: '1', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, fb147bae72727a9e8b7e9fc8c22e690345179367915f7d6c57c43e4c863ae48b: { Balance: '1606938044258990275541962092341162602522202993782792835301376', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } }, 3eb1ce4f84b7004d8c653aa4f5f7be9b54a240c293fbdd2553d1da13ceacf687: { Balance: '42000000000000000000', Nonce: 0, Root: '56e81f171bcc55a6ff8345e692c0f86e5b48e01b996cadc001622fb5e363b421', CodeHash: 'c5d2460186f7233c927e7db2dcc703c0e500b653ca82273b7bfad8045d85a470', Storage: {  } } } }
>
```
***

##### admin.import

    admin.import()

Imports the blockchain from a marshalled binary format.
**Note** that the blockchain is reset (to genesis) before the imported blocks are inserted to the chain.


**Returns** `true` on success, otherwise `false`.

```javascript
admin.import('path/to/file')
// true
```

***

##### admin.export

    admin.export()

Exports the blockchain to the given file in binary format.

**Returns** `true` on success, otherwise `false`.

```javascript
admin.export()
// binary ?
```

***

#### loadScript

     loadScript('/path/to/myfile.js');

Loads a JavaScript file and executes it.

***

##### web3
The `web3` exposes all methods of the [JavaScript API](https://github.com/ethereum/wiki/wiki/JavaScript-API).

***

##### net
The `net` is a shortcut for `web3.net`.

***

##### eth
The `eth` is a shortcut for `web3.eth`.

***

##### shh
The `shh` is a shortcut for `web3.shh`.

***

##### db
The `db` is a shortcut for `web3.db`.

***

## Examples 

### Balance

```javascript
coinbase = web3.eth.coinbase;
web3.eth.getBalance(coinbase).toNumber();
web3.eth.filter('pending').watch(function() {
  print(web3.eth.getBalance(coinbase).toNumber());
});
```

### Contract and transaction 
**note** solidity compilation doesn't work 

```javascript
var source = "" +
  "contract test {\n" +
  " /// @notice Will multiply `a` by 7. \n" +
  " function multiply(uint a) returns(uint d) {\n" +
  " return a * 7;\n" +
  " }\n" +
  "}\n";

var desc = [{
  "name": "multiply(uint256)",
  "type": "function",
  "inputs": [
  {
    "name": "a",
    "type": "uint256"
  }
  ],
    "outputs": [
    {
      "name": "d",
      "type": "uint256"
    }
  ]
}];
var address = web3.eth.sendTransaction({code: web3.eth.solidity(source)});
var contract = web3.eth.contract(address, desc);
var param = 3
contract.sendTransaction().multiply(param);
```