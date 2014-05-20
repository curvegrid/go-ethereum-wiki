The `ethereum` executable (not to be confused with `ethereal`) comes with a JavaScript console. The JS console can be invoked from the command line with the `-js` flag. **note** this flag disables all default logging to **stdout**. If you need log information, `tail -f $ETHEREUM/debug.log`.

It's also possible to use `ethereum` directly as a JavaScript intepreter. If you want to load a JavaScript file and invoke the VM directly execute `ethereum [options] <filename>`. 

## API

The JavaScript environment shares all bindings with the [Ext. JavaScript API](https://github.com/ethereum/go-ethereum/wiki/PoC-5-JavaScript-API).

The Ethereum API is available through the `eth` object and contains the following methods:

* `getBlock (hash)`
    Retrieves a block by either the address or the number. If supplied with a string it will assume address, number otherwise.
* `transact (sec, recipient, value, gas, gas price, data)`
    Creates a new transaction using your current key.
* `create (sec, value, gas, gas price, init, body)`
    Creates a new contract using your current key.
* `getKey ()`
    Retrieves your current key in hex format.
* `getStorageAt (object address, storage address)`
    Retrieves the storage address of the given object.
* `getBalanceAt (address)`
    Retrieves the balance at the current address
* `secretToAddress (sec)`
    Retrieves the address of the specified secret key.
* `getPeerCount()` 
    Returns the number of connected peers.
* `getIsMining()` 
    Returns whether or not the current node is mining.
* `getIsListening()` 
    Returns whether or not the current node is listening for connections.
* `getCoinBase()` 
    Returns the current coinbase address.
* `getTxCountAt()` 
    Returns the transaction nonce of the current key.
* `watch (address [, storage address], cb)`
    Watches for changes on a specific address' state object such as state root changes or value changes.
* `addPeer (host)`
    Make a connection to the specified host. Returns false if it didn't succeed.

## Loading modules

You can load **modules** by using the `require` method which can load other javascript files from disk and returns a special type of variable called `exports` (just like node.js does).

```javascript
exports.answer = 42;
```

and use it as:

```javascript
var answer = require('./myModuleWhichHasThe').answer;
```

***

#### Other

* `print (object)`
    Pretty prints the given object

Currently the event listening functions are disabled and not supported. This will likely be implemented in the near future.

* `disconnect (address [, storage address], cb)`
    Disconnects from a previous `watched` address.


#### Examples

```javascript
var inspect = require('sys').inspect;

eth.watch(eth.getAddress().address, function(stateObject) {
    inspect(stateObject)
});
```