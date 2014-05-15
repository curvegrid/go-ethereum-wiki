The `ethereum` executable (not to be confused with `ethereal`) comes with a JavaScript console. The JS console can be invoked from the command line with the `-js` flag. **note** this flag disables all default logging to **stdout**. If you need log information, `tail -f $ETHEREUM/debug.log`.

## API

The JS console shares all bindings with the [Ext. JavaScript API](https://github.com/ethereum/go-ethereum/wiki/PoC-5-JavaScript-API). The only difference is that the JS console has all methods starting with an **uppercase** character. (**might change**)

The Ethereum API is available through the `eth` object and contains the following methods:

* `GetBlock (hash)`
    Retrieves a block by either the address or the number. If supplied with a string it will assume address, number otherwise.
* `Transact (sec, recipient, value, gas, gas price, data)`
    Creates a new transaction using your current key.
* `Create (sec, value, gas, gas price, init, body)`
    Creates a new contract using your current key.
* `GetKey ()`
    Retrieves your current key in hex format.
* `GetStorageAt (object address, storage address)`
    Retrieves the storage address of the given object.
* `GetBalanceAt (address)`
    Retrieves the balance at the current address
* `SecretToAddress (sec)`
    Retrieves the address of the specified secret key.
* `GetPeerCount()` 
    Returns the number of connected peers.
* `GetIsMining()` 
    Returns whether or not the current node is mining.
* `GetIsListening()` 
    Returns whether or not the current node is listening for connections.
* `GetCoinBase()` 
    Returns the current coinbase address.
* `GetTxCountAt()` 
    Returns the transaction nonce of the current key.

Currently the event listening functions are disabled and not supported. This will likely be implemented in the near future.

* `Watch (address [, storage address], cb)`
    Watches for changes on a specific address' state object such as state root changes or value changes.
* `Disconnect (address [, storage address], cb)`
    Disconnects from a previous `watched` address.