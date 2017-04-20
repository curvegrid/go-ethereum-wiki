Dev diary Q2 2017 on the way to swarm POC 3 

### Binary Merkle Tree Hash (@zelig, @orenyodfat)

* https://github.com/ethereum/go-ethereum/pull/14334
* new bmt package to offer a suite of BMT hash implementations ready for base chunk hash for the swarm hash 
* BMT cleanup, let it go. https://github.com/ethereum/go-ethereum/pull/14334 slight controvercy around 

### Pyramid chunker (@jmozah, @holisticode)

* https://github.com/ethersphere/go-ethereum/pull/67
Priority 
* infinite (size-less) chunking
* append to hash (file)
* use SegmentWriter interface for chunk hash (provided by BMTHasher)

Next Phase

* obfuscation for plausible deniability: https://gist.github.com/nagydani/6ffc790c2e65af9c121489899e659b1d
* use infinite hash stream in 3rd gen syncer implementation 
* implement inclusion proofs and solidity verifiers for arbitrary files and segments

### network layer rewrite

* syncer
* streamer
* request handler

### interim syncer fix

* https://github.com/ethereum/go-ethereum/pull/3780 being tested on the testnet
* invalid chunks errors happening still see this comment  how to reproduce https://github.com/ethereum/go-ethereum/pull/3780#issuecomment-295449342
* I got a hunch, will update soon

### PSS (@nolash)

* https://github.com/ethersphere/go-ethereum/pull/68
* being rebased on 
* basic functionality and p2p protocol mount over PSS working
* adding simulation tests
* User Sceneario driven development of the PSS API



### Network testing and simulation framework (@lmars, @nolash, @homotopycolimit, @holisticode, @zelig)

* dev environment, local cluster within docker https://github.com/ethereum/go-ethereum/pull/14332/files
* local cluster https://github.com/ethereum/go-ethereum/pull/14353 
* add tests for cmd/swarm CLI functionality
* to provide the first real network type to interact with the simulation backend
* network scenario driver
* simplify code by removing messenger abstraction https://github.com/ethersphere/go-ethereum/pull/69
* https://gist.github.com/lmars/1461c8b51150613a595225753f0ff0d3

* event type refactor and visualiser improvements (@holisticode)

### API changes issues  (@homotopycolimit)

* https://github.com/ethereum/go-ethereum/issues/14349
* trailing slash strategy: http requests allow trailing slash
* arbitrary suffix / unique prefix 
* soft links as redirect 
* default entry as a manifest-wide 

*