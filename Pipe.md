# Ethereum Pipe API

General, easy to use, ethereum query interface.

```go
import "github.com/ethereum/eth-go/ethpipe"
```

Be aware that all methods return something. Nil isn't ever returned unless explicitly specified.

### Objects

* `Pipe`: The general Pipe query and information interface
* `world`: unexported world object through which you can query ethereum's state and objects.
* `config`: unexported config object through which you can query the `Config` contract if available.
* `object`: unexported object which functions as a proxy for `StateObject`. Returned by `config`

### Functions

* `New(ethchain.EthManager) *Pipe`: instantiate a new ethpipe object.

### `Pipe` Methods

* `World() *world`: returns the world object through which you can query ethereum's state.
* `Balance(address []byte) *Value`: returns the balance of the given `address`.
* `Nonce(address []byte) *uint64`: returns the the nonce of the given `address`.
* `Block(hash []byte) *Block`: returns the given block by `hash`.
* `Storage(address, storage []byte) *Value`: returns the given object by `address`'s value given by the `storage` address.
* `ToAddress(privateKey []byte) []byte`: converts a private key to an ethereum address.
* `Execute(address, data []byte, value, gas, price *Value) []byte`: Simulates an evaluation of the object's code given by the `address` and returns the outcome.
* `ExecuteObject(object *StateObject, data []byte, value, gas, price *Value) []byte`: Similar to the above only takes an actual `StateObject` instead of an address.

### `world` Methods

* `State() *State`: returns the current state of the ethereum `world` object.
* `Get(addres []byte) *StateObject`: returns the object given by the `address`. Returns `nil` if no object associated with the `address` can be found.
* `Config() *config`
* `IsListening() bool`: returns whether the client is listening for connections.
* `IsMining() bool`: returns whether the client is mining.
* `Peers() *list.List`: returns the current connected peers.
* `Coinbase() *StateObject`: TODO

### `config` Methods

* `Get(name string) object`: returns the associated object given by the `name`.
* `Exist() bool`: returns whether the config object exist in ethereum's present state.

### `object` Methods

* `StorageString(str string) *Value`: returns the storage value given by the key as `str` (Note, right pads zero to length of 32).
* `Storage(addr []byte)`: return the storage value given by the key as `address`.