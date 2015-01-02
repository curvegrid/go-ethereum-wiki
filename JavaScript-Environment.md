The `ethereum` executable (not to be confused with `mist`) comes with a JavaScript console. The JS console can be invoked from the command line with the `-js` flag. **note** this flag disables all default logging to **stdout**. If you need log information, `tail -f $ETHEREUM/debug.log`.

It's also possible to use `ethereum` directly as a JavaScript intepreter. If you want to load a JavaScript file and invoke the VM directly execute `ethereum [options] <filename>`. 

## API

The JavaScript REPL that comes with `ethereum` and it's API should not be confused with the [DApp JS API](https://github.com/ethereum/wiki/wiki/JavaScript-API). It tries to mimic the DApp API as much as possible except that private key accessibility is still possible. The intention of the REPL is very clear; utility. 

The Ethereum API is available through the `eth` object and contains the following methods:

* `block (hash)`
    Retrieves a block by either the address or the number. If supplied with a string it will assume address, number otherwise.
* `transact (sec, recipient, value, gas, gas price, data)`
    Creates a new transaction using your current key.
* `gxey ()`
    Retrieves your current key in hex format.
* `storageAt (object address, storage address)`
    Retrieves the storage address of the given object.
* `balanceAt (address)`
    Retrieves the balance at the current address
* `secretToAddress (sec)`
    Retrieves the address of the specified secret key.
* `peerCount()` 
    Returns the number of connected peers.
* `isMining()` 
    Returns whether or not the current node is mining.
* `isListening()` 
    Returns whether or not the current node is listening for connections.
* `coinbase()` 
    Returns the current coinbase address.
* `txCountAt()` 
    Returns the transaction nonce of the current key.
* `watch (address [, storage address], cb)`
    Watches for changes on a specific address' state object such as state root changes or value changes.
* `addPeer (host)`
    Make a connection to the specified host. Returns false if it didn't succeed.

The following methods are only available in the REPL and interpreter

### PStateObject

##### `getStateObject(address)`

The StateObject represents an object which has been stored in the state trie and can be queried for it's data

* `getStorage(address)`
    Returns the value stored at `address`
* `value()`
    Returns the value
* `address()`
    Returns the address
* `root()`
    Returns the root of the state object's state in hex
* `isContract()`
    Returns wether the state object is a contract or an account
* `script()`
    Returns the disassembled script.

### PKey

##### `key()`

The Key object is a key pair consisting of a private and public key from which an address can be derived.

* `privateKey`
   The private key of this key pair
* `publicKey`
   The public key of this key pair
* `address()`
   The derived address

### PBlock

##### `eth.getBlock(hash)`

* `getTransaction(hash)`
   Returns a transaction if the hash is valid, otherwise undefined

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
var inspect = eth.require('sys').inspect;

eth.watch(eth.getAddress().address, function(stateObject) {
    inspect(stateObject)
});
```