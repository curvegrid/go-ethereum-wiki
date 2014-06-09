## Currency

```go
this.store[this.origin()] = 10**20

return lambda {
    big to = this.data[0]
    big from = this.origin()
    big value = this.data[1]

    if this.store[from] > value {
        this.store[from] = this.store[from] - value
        this.store[to] = this.store[to] + value
    }
}

```

## Will and testament
```go
// The person who can claim the funds
#define CLAIMER 0xd766c288f24b91ae9781fe2b155d3260b8674c62
this.store[1000] = this.origin() // The person who wants to leave his funds to somebody if he dies a horrible dead

// Last I'm alive ping at:
this.store[1002] = this.time()

return lambda {
    // Function to prove you are still alive
    if this.store[1000] == this.origin() {
        this.store[1003] = this.time()

        // We want to send money from this contract for our own use.
        if this.data[0] == 1 {
            big data = this.data[3]
            transact(this.data[1], this.data[2], data)
        }
    }

    if this.origin() == CLAIMER {
        // I guess I'm dead; get the money out
        if this.time() - this.store[1003] > 60 * 60 * 24 * 60 {
            transact(CLAIMER, this.balance(), nil)
        }
    }
}
```