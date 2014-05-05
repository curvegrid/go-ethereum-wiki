The Ethereum JSON-RPC interface mimics the behaviour of the normal [javascript API](https://github.com/ethereum/go-ethereum/wiki/PoC-5-JavaScript-API). Most of the functions are implemented except for the **watch** and **disconnect** calls. Please note that all the arguments used in the RPC calls are named.

You can start the JSON-RPC server by suppling the `-r` flag to either `ethereum` or the `ethereal` client.

The examples in this documented are based upon node.js using [jsonrpc-tcp](https://www.npmjs.org/package/jsonrpc-tcp).

## RPC Calls

All RPC method calls are prefixed with EthereumApi. A call to GetKey would thus be `EthereumApi.GetKey`. The response object is always wrapped in a object that has an `error` key. If this key is set to `true` then please check `errorText` to read what kind of error has been produced.

Here is a list of the implemented RPC calls.

***
### EthereumApi.GetKey
Returns your private key details.

Arguments: none

Return data: address, privateKey, publicKey

**Example:**
```javascript
var jsonrpc = require('jsonrpc-tcp');
var client = jsonrpc.createClient();
client.connect(30304, function(remote) {
  remote.call("EthereumApi.GetKey", {}, function(err, result){
    console.log(result)
  })
}
```
```javascript
{"error":false,"result":{"address":"1dcb7190e038fbc13115ed1e2fbfbb732019ca4f","privateKey":"291b61e7aa4ac6cb7aab103be840c79961215744","publicKey":"041538a1ee93bf64f5dc4f6d1111d7ce0f025d4fd85a4268139272d88ffd3e3436394f6b106b34826f2e3ac024f319b393810c0042a219472f759e1103f4ac8d3"}}})
```

***

### Ethereum.GetBalanceAt

Retrieves the balance of a given address

Arguments: address

Return data: balance, address

**Example:**
```javascript
remote.call("EthereumApi.GetBalanceAt", {}, function(err, result){
  console.log(result)
})
```
```javascript
{"error":false,"result":{"balance":"1606938044258990275541962092341162602522202983782792835298976","address":"2ef47100e0787b915105fd5e3f4ff6752079d5cb"}}
```
***
### Ethereum.GetBlock
Retrieves the block information for a given hash

Arguments: hash

Return data: number, hash

**Example:**
```javascript
remote.call('EthereumApi.GetBlock', {"hash": "3afd62cec6b598dfc974c59a6f683671f9b8f60fe28ebb2f8a3cc942e2ed1d07"}, function(err, result) {
    console.log("result:",result);
  });
```
```javascript
{"error":false,"result":{"number":1,"hash":"3afd62cec6b598dfc974c59a6f683671f9b8f60fe28ebb2f8a3cc942e2ed1d07"}}
```
***
### Ethereum.GetStorageAt
Retrieves the storage address of the given object. 

Arguments: address, key

Return data: key, value address

**Example:**
```javascript
remote.call("EthereumApi.GetStorageAt", {"address": "16dfff98276b30655e825a09c9266a1ece888fde", "key":"2ef47100e0787b915105fd5e3f4ff6752079d5cb"}, function(err, result){
    console.log(result)
  })
```
```javascript
{"error":false,"result":{"key":"2ef47100e0787b915105fd5e3f4ff6752079d5cb","value":"100000000000000000000","address":"16dfff98276b30655e825a09c9266a1ece888fde"}}
```

***
### Ethereum.Transact
Creates a new transaction using your current key.

Arguments: recipient, value, gas, gasprice

Return data: createdContract, address, hash, sender

**Example:**
```javascript
remote.call('EthereumApi.Transact', {"recipient": "2ef47100e0787b915105fd5e3f4ff6752079d5cb", "value":"3","gas":"100", "gasprice": "200"}, function(err, result) {
    console.log("result:",result);
    console.log(err);
  });
```
```javascript
{"error":false,"result":{"createdContract":false,"address":"9f1f1501306a5821701056b9ff206a83a1aa7ddd","hash":"e9e694ee98fca14d0e89ddba9f1f1501306a5821701056b9ff206a83a1aa7ddd","sender":"2ef47100e0787b915105fd5e3f4ff6752079d5cb"}}
```
***
### Ethereum.Create
Creates a new contract using your current key.

Arguments: init, body, value, gas, gasprice

Return data: createdContract, address, hash, sender

**Example:**
```javascript
  remote.call('EthereumApi.Create', {"init": "store[this.Origin()] = 10^20", "body":"big to = this.Data(0) \
              big from = this.Origin() \
              big value = this.Data(1) \
              if store[from] > value { \
              store[from] = store[from] - value \
              store[to] = store[to] + value \
              }", "value":"300","gas":"1000000000", "gasprice": "20"}, function(err, result) {
   
    console.log(err);
});
```
```javascript
{"createdContract":false,"address":"16da9c5bac27936be6c90f9ca26be8c7325a854c","hash":"48c2b0086d3d6677412dc6ea16da9c5bac27936be6c90f9ca26be8c7325a854c","sender":"2ef47100e0787b915105fd5e3f4ff6752079d5cb"}}
```