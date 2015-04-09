# Running tests on go-ethereum
This page assumes go-ethereum has been configured according to the [Developers Guide](https://github.com/ethereum/go-ethereum/wiki/Developers'-Guide). All commands (unless stated otherwise) are assumed to be run from `$GOPATH/src/github.com/ethereum/go-ethereum`

## Unit tests
See [Travis](https://travis-ci.org/ethereum/go-ethereum/builds) or [Coveralls](https://coveralls.io/r/ethereum/go-ethereum) for status.

Test the full codebase locally by changing to the repository directory and running `test ./...`

## Integration tests
Integration tests for Go are included in the `tests` directory and can be run with standard go testing (i.e. `go test`). To run all the integration tests simply run:
```
go test ./tests/
```

### VM
[VM Test wiki](https://github.com/ethereum/tests/wiki/VM-Tests)

### State
[State Test wiki](https://github.com/ethereum/tests/wiki/State-tests

### Transaction
[Transaction Test wiki](https://github.com/ethereum/tests/wiki/Transaction-Tests)
```
go test ./tests/transaction_test.go
```

### Blockchain
[Blockchain Test wiki](https://github.com/ethereum/tests/wiki/Blockchain-Tests-II) 

### RPC
[RPC Tests repo](https://github.com/ethereum/rpc-tests)

1. Load test JSON with
    ```
    geth blocktest <pathToTheTestRepo>/BlockTests/bcJS_API_Test.json JS_API_Tests rpc
    ````
2. Run rpc-tests (https://github.com/ethereum/rpc-tests#usage)