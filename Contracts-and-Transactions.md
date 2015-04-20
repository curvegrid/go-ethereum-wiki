Currently being worked on.

Meanwhile see 
* https://github.com/ethereum/wiki/wiki/JavaScript-API#web3ethcontract
* https://github.com/ethereum/wiki/wiki/JSON-RPC

## Examples 

### Balance

```javascript
address = admin.accounts[0];
eth.getBalance(address).toNumber();
eth.filter('pending').watch(function() {
  print(eth.getBalance(address).toNumber());
});
```


