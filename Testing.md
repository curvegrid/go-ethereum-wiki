# Testing

## Unit tests
See [Travis](https://travis-ci.org/ethereum/go-ethereum/builds) or [Coveralls](https://coveralls.io/r/ethereum/go-ethereum) for status.

Test the full codebase locally by changing to the repository directory (`cd $GOPATH/src/github.com/ethereum/go-ethereum`) and running `test ./...`

## Integration tests

### VM
https://github.com/ethereum/tests/wiki/VM-Tests

### State
https://github.com/ethereum/tests/wiki/State-tests

### Transaction
See https://github.com/ethereum/tests/wiki/Transaction-Tests
`go test tests/transaction_test.go`

### Blockchain
See https://github.com/ethereum/tests/wiki/Blockchain-Tests and  https://github.com/ethereum/tests/wiki/Blockchain-Tests-II

### RPC

1. Install geth
2. Load test JSON `geth blocktest <pathToTheTestRepo>/BlockTests/bcJS_API_Test.json JS_API_Tests rpc`
3. Run rpc-tests (https://github.com/ethereum/rpc-tests#usage)