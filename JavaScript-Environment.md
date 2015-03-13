The `ethereum CLI` executable has a JavaScript console, which can be started with the `js` subcommand:

    $ ethereum js

If you need log information, start with:

    $ ethereum -logfile /tmp/eth.log -loglevel 5 js

It's also possible to pass files to the JavaScript intepreter. Load and execute any number of files by giving files as arguments to the `js` subcommand: 

    $ ethereum js script.0.js script.1.js

## Console API

Ethereum's javascript VM offers the full
 [DApp JS API](https://github.com/ethereum/wiki/wiki/JavaScript-API) by autoloading [ethereum.js](https://github.com/ethereum/ethereum.js).

Beside `web3`, its subcomponents are accessible directly from the global namespace as `eth`, `shh`, `db` and `net`.

* [admin](#admin)
  * [nodeInfo](#adminnodeinfo)
  * [addPeer](#adminaddpeer)
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

***

##### admin.addPeer

   admin.addPeer(nodeURL)

Pass a `nodeURL` to suggest a new peer to the network. The `nodeURL`need to be in a [enode url format](https://github.com/ethereum/wiki/wiki/enode-url-format).

You can find out your own enode by looking at the logs when the node boots up e.g.:

```
[P2P Discovery] Listening, enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303
```

**Returns** `true` on success.

```javascript
admin.addPeer('enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303')
```

***

##### admin.peers

   admin.peers()

**Returns** an array of objects with information about connected peers.

```javascript
admin.peers()
// [{...},{...}]
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

   admin.stopRpc()

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

Unlock the account for the time `timeout` in seconds.

**Returns** `true` on success, otherwise `false`.

```javascript
admin.unlock('0x833b8ed5a2957e5b050ba6539efa66cb67165eec', '1234', 1000 * 60 * 10) // unlock for 10 mins
// true
```

***

##### admin.newAccount

   admin.newAccount(password)

Creates a new account and encrypts it with `password`. If no `password` is given the key is stored unencrypted.

**Returns** `true` on success, otherwise `false`.

```javascript
admin.newAccount('1234')
// true
```

***

* `dumpBlock(numberOrHash)`
returns the raw dump of a block referred to by block number or block hash or undefined if the block is not found.


##### admin.dumpBlock

   admin.dumpBlock(numberOrHash)

**Returns** the raw dump of a block referred to by block number or block hash or undefined if the block is not found.

```javascript
admin.dumpBlock(29)
// {...}
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

##### web3
The `web3` exposes all the method of the [JavaScript API](https://github.com/ethereum/wiki/wiki/JavaScript-API).

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


## Loading modules (not implemented)

You can load **modules** by using the `require` method which can load other javascript files from disk and returns a special type of variable called `exports` (just like node.js does).

```javascript
exports.answer = 42;
```

and use it as:

```javascript
var answer = require('./myModuleWhichHasThe').answer;
```

relative paths in required modules are searched in `libPath`, a command line option. As a fallback `<path>.js` is tried. 
 
Once we got the module repository scheme using namereg and swarm, this is likely become much easier, like:

```javascript
require("bzz://ethrepo/ethereum/dapps/hivewallet/1.5.4")
require("bzz://github/ethersphere/faethbvk/tree/homestead/alpha/")
```

## Caveat 

The go-ethereum CLI uses the [Otto JS VM](https://github.com/obscuren/otto) (forked from https://github.com/robertkrimen/otto) which has some limitations:

* "use strict" will parse, but does nothing.
* The regular expression engine (re2/regexp) is not fully compatible with the ECMA5 specification.
* missing `setTimeout` and `setInterval`. These timing functions are not actually part of the ECMA-262 specification. Typically, they belong to the windows object (in the browser).

Since `ethereum.js` uses the [`bignumer.js`](https://github.com/MikeMcl/bignumber.js) library (MIT Expat Licence), it is also autoloded.

For scripting you may find useful the following implementation of sleep:

```javascript
function sleep(seconds) {
  var now = new Date().getTime();
  while(new Date().getTime() < now + seconds){}
}
```

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