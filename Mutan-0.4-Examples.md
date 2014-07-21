## Currency

```go
contract.storage[tx.sender()] = 10**20

return compile {
    var to = this.data[0]
    var from = tx.sender()
    var value = this.data[1]

    if contract.storage[from] > value {
        contract.storage[from] = contract.storage[from] - value
        contract.storage[to] = contract.storage[to] + value
    }
}
```

## Life Insurance

```go
#define CLAIMER 0xd766c288f24b91ae9781fe2b155d3260b8674c62 
this.store[1000] = tx.sender() 

func heartbeat() var {
    if contract.storage[1000] == tx.sender() {
        contract.storage[1002] = block.time()
        return true
    } else {
        if block.time() > contract.storage[1002] - 2592000 {
            return false
        } else {
            return true
        }
    }
}

func claim() var {
    if tx.sender() == CLAIMER {
        h := heartbeat()
        if h == false {
            transact(CLAIMER, balance(contract.address()), nil)
            return true
        } else {
            return false
        }
    }
}

func withdraw(var amount, var address) var {
    if contract.storage[1000] == tx.sender() {
        h := heartbeat()
        if h == true {
            return transact(address, amount, nil)
        } else {
            return false
        } 
    }
}

func run() {
    if contract.storage[1000] == tx.sender() {
        if this.data[0] == "heartbeat" {
            h := heartbeat()
            return h
        } else {
            address := this.data[1]
            amount := this.data[2]
            return withdraw(address, amount)
        }
    }

    if tx.sender() == CLAIMER {
        if this.data[0] == "claim" {
            c := claim()
            return c
        } else {
            return false
        }
    }
}
run()