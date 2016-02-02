# Tips 'n tricks

### Geth

#### Mining

* I only want to mine when there are transactions!

```javascript
function checkWork() {
	if( eth.getBlock("pending").transactions.length > 0 ) {
		if( !eth.mining ) miner.start(1);
	} else {
		miner.stop(0);
	}
}

setInterval(function() {
	checkWork();
}, 500);

eth.filter("latest", function(err, block) {
	checkWork();
});
```

#### Quick scripts

 * I want to get some data out without running a node!

```
$ geth --exec "eth.accounts" console 2>/dev/null

["0x0000000000000000000000000000000000000000"]
```

 * I want to get some data out without RPC magic!

```
$ geth --exec "eth.accounts" attach

["0x0000000000000000000000000000000000000000"]
```