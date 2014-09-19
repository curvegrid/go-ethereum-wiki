The PoC JavaScript API is a purposed API for all things JavaScript. JavaScript technologies can be embedded within Qt(QML) technologies, local web and remote web and therefor the purposed API is written in a ASYNC fashion so that it may be used across all implementations. Hereby it should be known that all functions, unless explicitly specified, take a callback as last function argument which will be called when the operation has been completed.

Please note that the PoC JavaScript API tries to leverage existing JS idioms as much as possible.

## Arguments

In general most method arguments take strings and can be in the following format:
* `"42"`
* `"0x2a"`

All of the above are interpreted as the number `42`. The JavaScript `String` object has the following extended methods in order to support proper bin/hex/decimal casting:


**Please note** that when passing numbers as arguments; pass them as strings instead. This to support large numbers without the need of extra type checking during argument conversion.

## Ethereum API

The Ethereum JS API implemented in Mist uses a **Future/Promises** pattern style for authoring HTML DApps. 
If you need a recap of **futures** have a look at [HTML5 Rocks JavaScript Promises](http://www.html5rocks.com/en/tutorials/es6/promises/).

All methods return promises so you can easily use them in your own code and chain them together.

* `coinbase` Returns the coinbase address
* `block(number_or_hash)` Returns the block with the given number or hash
* `transact(options)` Creates a new transaction with the given options (options them self may be promises):
  * `to` Address of the recipient
  * `from` Key for signing the transaction
  * `value` The endowment
  * `gas` Total amount of gas
  * `gasPrice` Gas priced to be used for this transaction
  * `data` Data or code for the transaction
* `compile(code)` Compiles the given code and returns the byte code (`mutan` and `serpent` supported)
* `key` Returns the default private key

### Examples

```javascript
var code = eth.compile("var y = 10")
    tx   = eth.transact({
                    from: eth.key(),
                    value: 10,
                    gas: 10000,
                    gasPrice: 10000000,
                    data: code,
            });
tx.then(function(tx) {
    console.log("Created tx", tx);
}).catch(error) {
    console.log("error tx", error);
});
```