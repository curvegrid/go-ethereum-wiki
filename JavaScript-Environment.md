The `ethereum CLI` executable (not to be confused with `mist`) comes with a JavaScript interpreter. The interpreter can be accesses via an interactive console (with persisted history). The JS console can be invoked from the command line with the `js` subcommand. If you need log information, start with (note the flags before the subcommand:

    ethereum -logfile /tmp/eth.log -loglevel 5 js

It's also possible to use the JavaScript intepreter with files so it gives a fully scriptable command line interface to the ethereum stack. Load and execute any number of files by giving files as arguments to the `js` subcommand: 

    ethereum js script.0.js script.1.js

## API

Ethereum's javascript VM offers the full
 [DApp JS API](https://github.com/ethereum/wiki/wiki/JavaScript-API) by autoloading [ethereum.js](https://github.com/ethereum/ethereum.js).
An admin interface is available via the `admin` object and contains the following methods:
**note** that the following is likely to change:

* `addPeer(nodeURL)`
tells the p2p server that we would like to dial out and connect to a specific peer.  Returns true if success, false there was an error. `nodeURL` is in [`enode`](https://github.com/ethereum/wiki/wiki/enode-url-format) format. You can find out your own from the logs when the client boots up (line to look for looks like:

    [P2P Discovery] Listening, enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303

Once you acquired the enode url of your favourite peer (say via whisper chat), to attempt connection you can use:

    admin.addPeer(" enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303")

* `removePeer(nodeId)` 
disconnects from a peer,  Returns true if success, false there was an error.

* `dumpBlock(numberOrHash)`
returns the raw dump of a block referred to by block number or block hash

* `import(filename)`
imports the blockchain from a marshalled binary format.  Returns true if success, false there was an error. Note that the blockchain is reset (to genesis) before the imported blocks are inserted to the chain.

* `export(filename)`
exports the blockchain to the given file in binary format. Returns true if success, false there was an error.

* `setMining(true|false)`
starts/stops mining returns true if mining, false if not mining.


## Loading modules

You can load **modules** by using the `require` method which can load other javascript files from disk and returns a special type of variable called `exports` (just like node.js does).

```javascript
exports.answer = 42;
```

and use it as:

```javascript
var answer = require('./myModuleWhichHasThe').answer;
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