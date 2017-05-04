Dev diary Q2 2017 on the way to swarm POC 3 

### Binary Merkle Tree Hash (@zelig, @orenyodfat)

* https://github.com/ethereum/go-ethereum/pull/14334
* new bmt package to offer a suite of BMT hash implementations ready for base chunk hash for the swarm hash 
* BMT cleanup, let it go. https://github.com/ethereum/go-ethereum/pull/14334 slight controvercy around 

Phase 1
* write tests for write (you can write to the hash writer arbitrary size blobs and still get the same result
* finalise and test the SegmentWriter interface to the Hasher
* define inclusion proofs on BMTHasher
* clean up, remove unused implementations

### Pyramid chunker (@jmozah, @holisticode)

2017-04-17
2017-04-24
Phase 0
* https://github.com/ethersphere/go-ethereum/pull/67
* infinite (size-less) chunking
* append to hash (file)
* https://github.com/ethereum/go-ethereum/pull/14382

2017-05-01
Phase 1
* add subtree length to root hash 
* use SegmentWriter interface for chunk hash (provided by BMTHasher)
* define a SegmentWriter wrapper for plain SHA3 chunkhash
* benchmark pyramid + SHA3 vs pyramid BMT

Phase 2
* append from arbitrary offset
* inclusion proofs and verifiers for arbitrary offset and length 

Phase 3
* obfuscation for plausible deniability: https://gist.github.com/nagydani/6ffc790c2e65af9c121489899e659b1d
* use infinite hash stream in 3rd gen syncer implementation 
* implement inclusion proofs and solidity verifiers for arbitrary files and segments (@nagydani)

### network layer rewrite
2017-04-17
* syncer
* streamer
* request handler

2017-05-01
* fixed kademlia async send and nil pointer bugs
* started to refactor swarm to be composed of a multiple apiservices and protocol modules


### interim syncer fix

2017-04-17
phase 0
* https://github.com/ethereum/go-ethereum/pull/3780 being tested on the testnet
* invalid chunks errors happening still see this comment  how to reproduce https://github.com/ethereum/go-ethereum/pull/3780#issuecomment-295449342

2017-04-24
phase 1
* the db bug finally fixed as of 26th April, also fixed syncer issues
* final review of https://github.com/ethereum/go-ethereum/pull/3780 and testing on testnet

2017-05-01
* http://swarm-gateways.net/bzz:/04772f473ba96a39b4f875d8fa22728691119a49cb981e9f6c22706ee3faf2d7/
* synced chunks cannot be retrieved, strange bug to hunt again

### PSS (@nolash)

2017-04-17
* https://github.com/ethersphere/go-ethereum/pull/68
* basic functionality and p2p protocol mount over PSS working
* adding simulation tests
* User Scenario driven development of the PSS API https://github.com/ethersphere/go-ethereum/wiki/PSS-usecases
* RPC API: https://gist.github.com/zelig/8d5f860f538a57fc7250af5b7c48d217 http://github.com/ethersphere/go-ethereum/tree/swam-pss-ws

2017-04-24
* https://github.com/ethersphere/go-ethereum/pull/68 reviewed and merged
* larger scale PSS only messaging routing correctness benchmarks
* proper API + documentation, see updated whisper 5 doc  https://github.com/ethereum/go-ethereum/wiki/Whisper
* (API client for p2p protocols using a p2p.MsgReadWriter implementation that ReadMsg via RPC subscribe and Write via SendRaw, handles peer via API also.

2017-05-01
* RPC Api done
* large scale tests (lil flaky)
* refactor, simplifications and imrovements

### Network testing and simulation framework (@lmars, @nolash, @homotopycolimit, @holisticode, @zelig)

2017-04-17
* dev environment, local cluster within docker https://github.com/ethereum/go-ethereum/pull/14332/files
* local cluster https://github.com/ethereum/go-ethereum/pull/14353 
* add tests for cmd/swarm CLI functionality
* to provide the first real network type to interact with the simulation backend
* network scenario driver https://gist.github.com/lmars/a0b90f0208b153ff313d9e0a1aeb3258
* simplify code by removing messenger abstraction https://github.com/ethersphere/go-ethereum/pull/69
* https://gist.github.com/lmars/1461c8b51150613a595225753f0ff0d3
* event type refactor and visualiser improvements (@holisticode) https://github.com/holisticode/simple-p2p-d3/tree/snapshotsUX

2017-04-24
* Added integration tests for the swarm CLI: https://github.com/ethereum/go-ethereum/pull/14353.
  These are similar to the geth integration tests, executing the current running binary but with
  a hook to run the CLI app rather than the tests, which avoids the need to compile a separate
  binary.

* Added an "exec" adapter to the network simulation framework which supports running protocols
  using the devp2p stack as child processes: https://github.com/ethersphere/go-ethereum/pull/70.

* Added a "docker" adapter which is similar to the exec adapter but runs the nodes in
  docker containers: https://github.com/ethersphere/go-ethereum/pull/71.
  That PR also includes a change to do RPC over stdin / stdout (so we don't need to access the
  node's filesystem or TCP stack).
* Refactor the in-process adapter to use RPC to provide a consistent API across all adapters
  (this means you can just pass a `node.Service` to any adapter, be it in-process, exec, docker
  or flynn).

Upcoming:

* Add a "flynn" adapter which runs nodes in a [Flynn cluster](https://flynn.io/). This will allow
  us to spin up clusters in Azure / AWS / GCP / bare metal using the Flynn installer, run some
  simulations then spin the cluster back down again.

* Add some hooks for storing benchmarks and metrics in a time-series database (perhaps
  [Prometheus](https://prometheus.io/)).

2017-05-01
* preparing for the GRAND PR 
* refactor
* event type  refactor

### Light swarm nodes

Specing out light swarm node functionalities and roadmap
https://github.com/ethereum/go-ethereum/issues/14385

### API changes issues  (@homotopycolimit)

* https://github.com/ethereum/go-ethereum/issues/14349
* trailing slash strategy: http requests allow trailing slash
* arbitrary suffix / unique prefix 
* soft links as redirect 
* default entry as a manifest-wide 
* service specific blockchain client endpoints: https://github.com/ethereum/go-ethereum/issues/14386

### Documentation

* update the swarm guide to include documentation for the swarm manifest, ls subcommands, and most importantly the extended upload/download options (multipart, tar stream) 
* upload/download notes by @lmars https://gist.github.com/lmars/a37f3eaa129f95273c8c536e98920368
* swarm/dev env docs under swarm/dev/README

### markdown editor dapp

http://swarm-gateways.net/bzz:/8a300084e9b3be0d0a06169af56346a29f8d440b4a2fceef57cf14d5202482fb/#02e8ec3b6faa879d6288c2b8a2f4053ac21da328a92b2dfab0e8f6be3f343a79