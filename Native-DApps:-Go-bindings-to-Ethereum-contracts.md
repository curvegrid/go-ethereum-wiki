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
also available at [token.sol](https://gist.github.com/karalabe/08f4b780e01c8452d989).

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
$ abigen --abi token.abi --pkg main --out token.go
```

Where the flags are:

 * `--abi`: Mandatory path to the contract ABI to bind to
 * `--pgk`: Mandatory Go package name to place the Go code into
 * `--out`: Optional output path for the generated Go source file (not set = stdout)

This will generate a type-safe Go binding for the Token contract. The generated code will
look something like [`token.go`](https://gist.github.com/karalabe/52d08143c4c1dfa8971a), but
please generate your own as this will change as more work is put into the generator.