The PoC JavaScript API is a purposed API for all things JavaScript. JavaScript technologies can be embedded within Qt(QML) technologies, local web and remote web and therefor the purposed API is written in a ASYNC fashion so that it may be used across all implementations. Hereby it should be known that all functions, unless explicitly specified, take a callback as last function argument which will be called when the operation has been completed.

Please note that the PoC JavaScript API tries to leverage existing JS idioms as much as possible.

## Arguments

In general most method arguments take strings and can be in the following format:
* `"*"`
* `"42"`
* `"0x2a"`

All of the above are interpreted as the number `42`. The JavaScript `String` object has the following extended methods in order to support proper bin/hex/decimal casting:

* `bin` casts the current string to binary `"0x2a".bin() // => "*"`
* `unbin` casts the current binary string to hex format `"*".unbin() // => "0x2a"`
* `pad(len)` casts the current string to binary format and left-pad zeros until `len` is satisfied.
* `unpad` removes all left-padded zeros.

**Please note** that when passing numbers as arguments; pass them as strings instead. This to support large numbers without the need of extra type checking during argument conversion.

## Ethereum API

The Ethereum API is available through the `eth` object and contains the following methods:

* `getBlock (number or string)`
    Retrieves a block by either the address or the number. If supplied with a string it will assume address, number otherwise.
* `transact (sec, recipient, value, gas, gas price, data)`
    Creates a new transaction using your current key.
* `create (sec, value, gas, gas price, init, body)`
    Creates a new contract using your current key.
* `getKey (none)`
    Retrieves your current key in hex format.
* `getStorageAt (object address, storage address)`
    Retrieves the storage address of the given object.
* `getBalanceAt (object address)`
    Retrieves the balance at the current address
* `watch (string [, string])`
    Watches for changes on a specific address' state object such as state root changes or value changes.
* `disconnect (string [, string])`
    Disconnects from a previous `watched` address.
* `secretToAddress (seckey)`
    Retrieves the address of the specified secret key.

## Events

Events are supported through a jQuery inspired mechanism.

* `on (event)`
    Subscribe to event which will be called whenever an event of type <event> is received.
* `off (event)`
    Unsubscribe to the given event
* `trigger (event, data)`
    Trigger event of type <event> with the given data. **note:** This function does not take a callback function.
    
### Event Types

All events are written in camel cased style beginning with a lowercase letter. Subevents are denoted by a colon `:`.

* `block:new`
    Fired when a new valid block has been found on the wire. The attached value of this call is a block.

## Window settings

The JavaScipt `window` object references the actual GUI window and therefor changes you make to the `window` object will have an impact on the GUI window.

You can change the title by setting the document's title `window.document.title = "Hello world"`. If you'd like to resize the window to a specific dimension you can use `window.resizeTo(600, 300)`. Please refer to the [JavaScript window object](https://developer.mozilla.org/en/docs/Web/API/Window) for more options.

## Examples

#### Key pair

```html
<div>Your current key is: <strong id="key"></strong>.</div>
<script>
eth.getKey(function(privateKey) {
        document.querySelector("#key").innerHTML = privateKey;
});
</script>
```

#### Transact

```html
<div id="tx-hash"></div>
<script>
var data = ("42".pad(32) + (7 + 2).toString().pad(32)).unbin()
eth.transact(privateKey, myContractAddr, "0", "10000", "100", data, function(receipt) {
        document.querySelector("tx-hash").innerHTML = receipt.hash;
});
</script>
```