## Currency

```go
init {
	store[this.origin()] = 10^20
}

main {
	big to = this.data[0]
	big from = this.origin()
	big value = this.data[1]

	if store[from] > value {
		store[from] = store[from] - value
		store[to] = store[to] + value
	}
}
```

## Will and testament
```go
init {
  this.store[1000] = this.origin() // The person who wants to leave his funds to somebody if he dies a horrible dead
  this.store[1001] = 0xd766c288f24b91ae9781fe2b155d3260b8674c62 // The person who can claim the funds

  // Last I'm alive ping at:
  this.store[1002] = this.time()
}

main {
  // Function to prove you are still alive
  if this.store[1000] == this.origin() {
    this.store[1003] = this.time()

    // We want to send money from this contract for our own use.
   if this.data[0] == 1 {
     big data = this.data[3]
     transact(this.data[1], this.data[2], data)
    }
  }

  if this.origin() == this.store[1001]{
    // I guess I'm dead; get the money out
    if this.time() - this.store[1003] > 60 * 60 * 24 * 60{
      transact(this.store[1001], this.balance(), nil)
    }
  }
}

```