The original roadmap and/or dream of the Ethereum platform was to provide a solid, high
performing client implementation of the consensus protocol in various languages, which
would provide an RPC interface for JavaScript DApps to communicate with, pushing towards
the direction of the Mist browser, through which users can interact with the blockchain.

Although this was a solid plan for mainstream adoption and does cover quite a lot of use
cases that people come up with (mostly where people manually interact with the blockchain),
it eludes the server side (backend, fully automated, devops) use cases where JavaScript is
usually not the language of choice given its dynamic nature.

This page introduces the concept of server side native Dapps: Go language bindings to any
Ethereum contract that is compile time type safe, highly performant and best of all, can
be generated fully automatically from a contract ABI and optionally the EVM bytecode.

*This page is written in a more beginner friendly tutorial style to make it easier for
people to start out with writing Go native Dapps. The used concepts will be introduced
gradually as a developer would need/encounter them. However, we do assume the reader
is familiar with Ethereum in general, has a fair understanding of Solidity and can code
Go.*

## Token contract 

To avoid falling into the fallacy of useless academic examples, we're going to take the
official [Token contract](https://ethereum.org/token) as the base for introducing the Go
native bindings. If you're unfamiliar with the contract, skimming the linked page should
probably be enough, the details aren't relevant for now. *In short the contract implements
a custom token that can be deployed on top of Ethereum.* To make sure this tutorial doesn't
go stale if the linked website changes, the Solidity source code of the Token contract is
also available at [`token.sol`](https://gist.github.com/karalabe/08f4b780e01c8452d989).

### Go binding generator

Interacting with a contract on the Ethereum blockchain from Go (or any other language for
a matter of fact) is already possible via the RPC interfaces exposed by Ethereum clients.
However, writing the boilerplate code that translates decent Go language constructs into
RPC calls and back is extremely time consuming and also extremely brittle: implementation
bugs can only be detected during runtime and it's almost impossible to evolve a contract
as even a tiny change in Solidity can be painful to port over to Go.

To avoid all this mess, the go-ethereum implementation introduces a source code generator
that can convert Ethereum ABI definitions into easy to use, type-safe Go packages. Assuming
you have a valid Go development environment set up, `godep` installed and the go-ethereum
repository checked out correctly, you can build the generator with:

```
$ cd $GOPATH/src/github.com/go-ethereum
$ godep go install ./cmd/abigen
```

### Generating Go bindings

The single essential thing needed to generate a Go binding to an Ethereum contract is the
contract's ABI definition `JSON` file. For our `Token` contract tutorial you can obtain this
either by compiling the Solidity code yourself (e.g. via @chriseth's [online Solidity compiler](https://chriseth.github.io/browser-solidity/)), or you can download our pre-compiled [`token.abi`](https://gist.github.com/karalabe/b8dfdb6d301660f56c1b).

To generate a binding, simply call:

```
$ abigen --abi token.abi --pkg main --type Token --out token.go
```

Where the flags are:

 * `--abi`: Mandatory path to the contract ABI to bind to
 * `--pgk`: Mandatory Go package name to place the Go code into
 * `--type`: Optional Go type name to assign to the binding struct
 * `--out`: Optional output path for the generated Go source file (not set = stdout)

This will generate a type-safe Go binding for the Token contract. The generated code will
look something like [`token.go`](https://gist.github.com/karalabe/52d08143c4c1dfa8971a), but
please generate your own as this will change as more work is put into the generator.

### Accessing an Ethereum contract

To interact with a contract deployed on the blockchain, you'll need to know the `address`
of the contract itself, and need to specify a `backend` through which to access Ethereum.
The binding generator provides out of the box an RPC backend through which you can attach
to an existing Ethereum node via IPC, HTTP or WebSockets.

We'll use the foundation's [Unicorn](https://ethereum.org/donate) token contract deployed
on the testnet to demonstrate calling contract methods. It is deployed at the address
`0x21e6fc92f93c8a1bb41e2be64b4e1f88a54d3576`.

```go
package main

import (
	"fmt"
	"log"

	"github.com/ethereum/go-ethereum/accounts/abi/bind/backends"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/rpc"
)

func main() {
	// Create an IPC based RPC connection to a remote node
	conn, err := rpc.NewIPCClient("/home/karalabe/.ethereum/testnet/geth.ipc")
	if err != nil {
		log.Fatalf("Failed to connect to the Ethereum client: %v", err)
	}
	// Instantiate the contract and display its name
	token, err := NewToken(common.HexToAddress("0x21e6fc92f93c8a1bb41e2be64b4e1f88a54d3576"), backends.NewRPCBackend(conn))
	if err != nil {
		log.Fatalf("Failed to instantiate a Token contract: %v", err)
	}
	name, err := token.Name(nil)
	if err != nil {
		log.Fatalf("Failed to retrieve token name: %v", err)
	}
	fmt.Println("Token name:", name)
}
```

And the output (yay):

```
Token name: Testnet Unicorn
```

If you look at the method invoked to read the token name `token.Name(nil)`, it required
a parameter to be passed, even though the original Solidity contract requires none. This
is a `*bind.CallOpts` type, which can be used to fine tune the call.

 * `Pending`: Whether to access pending contract state or the current stable one
 * `GasLimit`: Place a limit on the computing resources the call might consume

### Transacting with an Ethereum contract

Invoking a method that changes contract state (i.e. transacting) is a bit more involved,
as a live transaction needs to be authorized and broadcast into the network. **Opposed
to the conventional way of storing accounts and keys in the node we attach to, Go bindings
require signing transactions locally and do not delegate this to a remote node.** This is
done so to facilitate the general direction of the Ethereum community where accounts are
kept private to DApps, and not shared (by default) between them.

Thus to allow transacting with a contract, your code needs to implement a method that
given an input transaction, signs it and returns an authorized output transaction. Since
most users have their keys in the [Web3 Secret Storage](https://github.com/ethereum/wiki/wiki/Web3-Secret-Storage-Definition) format, the `bind` package contains a small utility method 
(`bind.NewTransactor(keyjson, passphrase)`) that can create an authorized transactor from
a key file and associate password, without the user needing to implement key signing himself.

Changing the previous code snippet to send one unicorn to the zero address:

```go
package main

import (
	"fmt"
	"log"
	"math/big"

	"github.com/ethereum/go-ethereum/accounts/abi/bind"
	"github.com/ethereum/go-ethereum/accounts/abi/bind/backends"
	"github.com/ethereum/go-ethereum/common"
	"github.com/ethereum/go-ethereum/rpc"
)

const key = `paste the contents of your *testnet* key json here`

func main() {
	// Create an IPC based RPC connection to a remote node and instantiate a contract binding
	conn, err := rpc.NewIPCClient("/home/karalabe/.ethereum/testnet/geth.ipc")
	if err != nil {
		log.Fatalf("Failed to connect to the Ethereum client: %v", err)
	}
	token, err := NewToken(common.HexToAddress("0x21e6fc92f93c8a1bb41e2be64b4e1f88a54d3576"), backends.NewRPCBackend(conn))
	if err != nil {
		log.Fatalf("Failed to instantiate a Token contract: %v", err)
	}
	// Create an authorized transactor and spend 1 unicorn
	auth, err := bind.NewTransactor(key, "my awesome super secret password")
	if err != nil {
		log.Fatalf("Failed to create authorized transactor: %v", err)
	}
	tx, err := token.Transfer(auth, common.HexToAddress("0x0000000000000000000000000000000000000000"), big.NewInt(1))
	if err != nil {
		log.Fatalf("Failed to request token transfer: %v", err)
	}
	fmt.Printf("Transfer pending: 0x%x\n", tx.Hash())
}
```

And the output (yay):

```
Transfer pending: 0x4f4aaeb29ed48e88dd653a81f0b05d4df64a86c99d4e83b5bfeb0f0006b0e55b
```

*Note, with high probability you won't have any testnet unicors available to spend, so the
above program will fail with an error. Send at least 2.014 ethers to the foundation testnet
donation account `0xDf7D0030bfed998Db43288C190b63470c2d18F50` to receive a unicorn token and
you'll be able to see the above code run without an error!*

Similar to the method invocations in the previous section which only read contract state,
transacting methods also require a mandatory first parameter to be passed, a `*bind.TransactOpts`
type, which authorizes the transaction and potentially fine tunes it:

 * `Account`: Address of the account to invoke the method with (mandatory)
 * `Signer`: Method to sign an transaction locally before broadcasting it (mandatory)
 * `Nonce`: Account nonce to use for the transaction ordering (optional)
 * `GasLimit`: Place a limit on the computing resources the call might consume (optional)
 * `GasPrice`: Explicitly set the gas price to run the transaction with (optional)
 * `Value`: Any funds to transfer along with the method call (optional)

The two mandatory fields are automatically set by the `bind` package if the auth options are
constructed using `bind.NewTransactor`. The nonce and gas related fields are automatically
derived by the binding if they are not set. An unset value is assumed to be zero.

### Authorized sessions

### Deploying new instances of a contract