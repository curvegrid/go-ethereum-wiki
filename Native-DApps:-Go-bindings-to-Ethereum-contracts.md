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
gradually as a developer would need/encounter them. We however, do assume that the reader
is familiar with Ethereum in general, has a fair understanding of Solidity and can code
Go.*