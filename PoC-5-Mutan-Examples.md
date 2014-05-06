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