# Ethereum Pipe API

General, easy to use, ethereum query interface. This API allows you to easily interface with ethereum's state and their respective objects, create transactions and directly evaluate contracts.

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
* `Transact(key *KeyPair, address []byte, value, gas, price *Value, data []byte) error`: creates a new transaction using the given `key`.

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

## Example

```go
import "github.com/ethereum/eth-go/ethpipe"

pipe := ethpipe.New(ethereum)

var addr, privy, recp, data []byte
var object *ethstate.StateObject
var key *ethcrypto.KeyPair

world := pipe.World()
world.Get(addr)
world.Coinbase()
world.IsMining()
world.IsListening()
world.State()
peers := world.Peers()
peers.Len()

// Shortcut functions
pipe.Balance(addr)
pipe.Nonce(addr)
pipe.Block(addr)
pipe.Storage(addr, addr)
pipe.ToAddress(privy)
// Doesn't change state
pipe.Execute(addr, nil, Val(0), Val(1000000), Val(10))
// Doesn't change state
pipe.ExecuteObject(object, nil, Val(0), Val(1000000), Val(10))

conf := world.Config()
namereg := conf.Get("NameReg")
namereg.Storage(addr)

var err error
// Transact
err = pipe.Transact(key, recp, ethutil.NewValue(0), ethutil.NewValue(0), ethutil.NewValue(0), nil)
if err != nil {
	t.Error(err)
}
// Create
err = pipe.Transact(key, nil, ethutil.NewValue(0), ethutil.NewValue(0), ethutil.NewValue(0), data)
if err != nil {
	t.Error(err)
}
```