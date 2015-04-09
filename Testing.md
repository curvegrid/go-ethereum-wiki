# Testing

## Unit tests
See [Travis](https://travis-ci.org/ethereum/go-ethereum/builds) or [Coveralls](https://coveralls.io/r/ethereum/go-ethereum) for status.

Test the full codebase locally by changing to the repository directory (`cd $GOPATH/src/github.com/ethereum/go-ethereum`) and running `test ./...`

## Integration tests
Integration tests for Go are included in the `tests` directory and can be run with standard go testing (i.e. `go test`). To run all the integration tests simply run:
```
cd $GOPATH/src/github.com/ethereum/go-ethereum && go test ./tests/
```

### VM
[VM Test wiki](https://github.com/ethereum/tests/wiki/VM-Tests)

### State
[State Test wiki](https://github.com/ethereum/tests/wiki/State-tests

### Transaction
[Transaction Test wiki](https://github.com/ethereum/tests/wiki/Transaction-Tests)
Run with `go test ./tests/transaction_test.go`

### Blockchain
[Blockchain Test wiki](https://github.com/ethereum/tests/wiki/Blockchain-Tests-II) 

### RPC
[RPC Tests repo](https://github.com/ethereum/rpc-tests)

1. Install geth
2. Load test JSON with `geth blocktest <pathToTheTestRepo>/BlockTests/bcJS_API_Test.json JS_API_Tests rpc`
3. Run rpc-tests (https://github.com/ethereum/rpc-tests#usage)