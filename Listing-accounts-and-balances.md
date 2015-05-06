# Checking balances

### Listing your current accounts

From the command line, call the CLI with:
```
$ geth account list

Primary #0: {d1ade25ccd3d550a7eb532ac759cac7be09c2719}
Account #1: {da65665fc30803cb1fb7e6d86691e20b1826dee0}
Account #2: {e470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32}
Account #3: {f4dd5c3794f1fd0cdc0327a83aa472609c806e99}
```

when using the console:
```
> eth.accounts
['0x407d73d8a49eeb85d32cf465507dd71d507100c1']
```

or via RPC:
```
# Request
$ curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1} http://127.0.0.1:8545'

# Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
}
```

### Checking account balances

To check your the etherbase account balance:
```
> web3.fromWei(eth.getBalance(eth.coinbase), "ether")
6.5
```

Print all balances with a JavaScript function:
```
function checkAllBalances() { 
var i =0; 
eth.accounts.forEach( function(e){
 	console.log("  eth.accounts["+i+"]: " +  e + " \tbalance: " + web3.fromWei(eth.getBalance(e), "ether") + " ether"); 
i++; 
})
}; 
```
That can then be executed with:
```
> checkAllBalances();
  eth.accounts[0]: 0xd1ade25ccd3d550a7eb532ac759cac7be09c2719 	balance: 63.11848 ether
  eth.accounts[1]: 0xda65665fc30803cb1fb7e6d86691e20b1826dee0 	balance: 0 ether
  eth.accounts[2]: 0xe470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32 	balance: 1 ether
  eth.accounts[3]: 0xf4dd5c3794f1fd0cdc0327a83aa472609c806e99 	balance: 6 ether
```