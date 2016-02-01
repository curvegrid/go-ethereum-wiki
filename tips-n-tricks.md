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