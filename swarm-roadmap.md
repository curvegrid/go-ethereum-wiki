# swarm roadmap 

In brief we group the tasks under various tracks that roughly correspond to subpackages of the swarm codebase

* api
* network
* storage
* services
* examples
* testing
* documentation

The features and enhancements below will be prioritised and for the basis of the various POC sprints starting Q2 2016

## Testing

### Stages of network (testing)

* [ ] see that private network is working correctly
* [ ] see that the private network is working and benchmarking the influence of block propagation gives satisfying results
* [ ] an upgraded swarm on a private network is opened up for public participation
* [ ] the private test network with community gives us another order of magnitude bigger network to test on
* [ ] homestead-compatible swarm is joining the Morden testnet
* [ ] swarm integrated in Mist and allowed on the main net
* [ ] with POC 0.2 homestead-compatible on a toy testnet 
* [ ] continue to expand and open to the public
* [ ] abstract network simulation framework using shadow



## network

* [x] scripted network tests, cluster control framework (to be obsoleted by eth/
* [x] algorithmic improvements on chunker split/join
* [x] algorithmic improvements on upload/download
* [x] algorithmic improvements in kademlia and hive peers manager 
* [x] calibrating kademlia connectivity parameters to toynet scale
* [ ]  adapt to [felix's rpc-client-as-eth-backend scheme](http://twurst.com/articles/geth-1.5-api.html) to run swarm as a separate daemon 
* [ ] deliveries should not enter the sync pool - enhancement [#2046](https://github.com/ethereum/go-ethereum /issues/2046)
* [ ] smart peer propagation enhancement feature [#2044](https://github.com/ethereum/go-ethereum/issues/2044)
* [ ] IPFS integration: [IPFS & SWARM](https://github.com/ethereum/go-ethereum/wiki/IPFS-&-SWARM)  feature [#2050](https://github.com/ethereum/go-ethereum/issues/2050)
* [ ] abstract out hive kademlia  routing from protocol
* [ ] enhanced network monitoring, structured logging and stats aggregation
* [ ] unicast/multicast messaging over swarm - pss 
* [ ] improved peer propagation [#2044](https://github.com/ethereum/go-ethereum/issues/2044)
* [ ] protocol stack abstraction, pluggable subprotocol components - (swap, hive/protocol, syncer, peers) for pss
* [ ] new p2p API integration - feature [#2060](https://github.com/ethereum/go-ethereum/issues/2060)

## storage
* [ ] syncdb/netstore rewrite
* [ ] push on delete - feature [#2045](https://github.com/ethereum/go-ethereum/issues/2045)
* [ ] storage parametera setting API
* [ ] storage monitoring  for Mist swarm dashboard
* [ ] integrate smash hash and swarm hash
* [ ] rewrite chunk size encoding
* [ ] basic masking - streaming-capable per-chunk encryption 
* [ ] uri based masking (plausible deniability)
* [ ] Cauchy-Reed-Solomon erasure coding on chunker - see [sw^3 - orange paper 1](http://swarm-gateways.net/bzz:/swarm/ethersphere/orange-papers/1/sw^3.pdf)
* [ ] 

## API
* [ ] bzz protocol should implement info for reporting - feature [#2042](https://github.com/ethereum/go-ethereum/issues/2042)
* [ ] chunker and document level API improvements
  * [ ] manifest support for CRS and Encryption
  * [ ] poor man's DRM and copyright protection
  * [ ] integrity checking API
  * [ ] swear registration API for Mist swarm dashboard
  * [ ] service management API for Mist swarm dashboard
* [ ] improved file system support
  * [ ] skeleton manifests [#2437](https://github.com/ethereum/go-ethereum/issues/2437)
  * [ ] file system watcher - dropbox backend API
* [ ] remove [BZZ] tag from logs swarm - enhancement [#2345](https://github.com/ethereum/go-ethereum/issues/2345)
* [x] support for separate url schemes for dns enabled, immutable and raw manifest - feature [#2279](https://github.com/ethereum/go-ethereum/issues/2279)

## services

* [ ] ENS, ethereum name service
  * [x] [ENS ERC by @Arachnid](https://github.com/Arachnid/EIPs/blob/ens/EIPS/eip-draft-ens.md)
  * [x] swarm ENS - enhancement [#2422](https://github.com/ethereum/go-ethereum/issues/2422)
  * [ ] implementation for [EIP-26](https://github.com/ethereum/EIPs/issues/26)
  * [ ] swarm namereg/natspec rewrite - enhancement [#2048](https://github.com/ethereum/go-ethereum/issues/2048)
* [ ] Incentives
  * [ ] improve [swap accounting](https://github.com/ethersphere/swarm/wiki/Swap) with debt swaps and liquidity check
  * [ ] implement storage insurance as per [_swap, swear and swindle_ incentive scheme](http://swarm-gateways.net/bzz:/swarm/ethersphere/orange-papers/1/sw^3.pdf)
  * [ ] swear and swindle solidity contracts
  * [ ] smash proofing [smash - orange paper 2](http://swarm-gateways.net/bzz:/swarm/ethersphere/orange-papers/2/smash.pdf) + solidity contract
  * [ ] automated audits with crash - collection-level recursive audit secret hash
  * [ ] throughput monitoring for improved peer ranking
  * [ ] price discovery and dynamic pricing 
* [ ] sword
  * [ ] swarm DB support phase 0 - compact manifest trie and proof requests
  * [ ] [SWarm On-demand Retrieval Daemon](https://gist.github.com/zelig/aa6eb43615e12d834d9f) - feature [#2049](https://github.com/ethereum/go-ethereum/issues/2049) = sword. ethereum state, contract storage, receipts, blocks on swarm

## examples

* [ ] photo album
* [ ] file manager
* [ ] dropbox
* [ ] swatch
* [ ] git/github
* [ ] tubee

