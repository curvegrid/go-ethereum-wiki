# swarm roadmap 

## POC0.1: sworm

* [x] underground dev testnet launched on BZZ day (3.22 is l33t for BZZ)
* [x] create ansible and docker for node deployment
* [x] a few public gateways 

## POC0.2 Homestead sworm 

* [x] rebase on geth 1.5 

## Release of content 

* [x] orange paper series released on swarm 
* [x] landing page for swarm with some minimal info

## Stages of network (testing)

* [ ] see that private network is working correctly
* [ ] see that the private network is working and benchmarking the influence of block propagation gives satisfying results
* [ ] an upgraded swarm on a private network is opened up for public participation
* [ ] the private test network with community gives us another order of magnitude bigger network to test on
* [ ] homestead-compatible swarm is joining the Morden testnet
* [ ] swarm integrated in Mist and allowed on the main net

## Issues to be resolved before feature tasks

* [ ] finish scripted network tests 
* [ ] syncer, forwarder, depo unit tests
* [ ] remove [BZZ] tag from logs swarm - enhancement [#2345](https://github.com/ethereum/go-ethereum/issues/2345)
* [x] support for separate url schemes for dns enabled, immutable and raw manifest - feature [#2279](https://github.com/ethereum/go-ethereum/issues/2279)
* [ ] deliveries should not enter the sync pool - enhancement [#2046](https://github.com/ethereum/go-ethereum/issues/2046)
* [ ] push on delete - feature [#2045](https://github.com/ethereum/go-ethereum/issues/2045)
* [ ] smart peer propagation enhancement feature [#2044](https://github.com/ethereum/go-ethereum/issues/2044)

## Features

The features and enhancements below will be prioritised and for the basis of the various POC sprints starting Q2 2016

* [ ] Mist integration 
  * [ ] storage monitoring and parameter setting API for Mist swarm dashboard
  * [ ] bzz protocol should implement info for reporting - feature [#2042](https://github.com/ethereum/go-ethereum/issues/2042)
  * [ ] swear registration API for Mist swarm dashboard
  * [ ] service management API for Mist swarm dashboard
  * [ ] swap reputation support - reputation and peer stats API for Mist swarm dashboard
* [ ] DNS and contract registrar rewrite 
  * [x] [DNS ERC by @Arachnid](https://github.com/Arachnid/EIPs/blob/ens/EIPS/eip-draft-ens.md)
  * [ ] swarm DNS - enhancement [#2422](https://github.com/ethereum/go-ethereum/issues/2422)
  * [ ] implementation for [EIP-26](https://github.com/ethereum/EIPs/issues/26)
  * [ ] swarm namereg/natspec rewrite - enhancement [#2048](https://github.com/ethereum/go-ethereum/issues/2048)
    * [ ] distributed indexing
* [ ] chunker and document level API improvements
  * [ ] integrate smash hash and swarm hash
  * [ ] rerwite chunk size encoding
  * [ ] basic masking - streaming-capable per-chunk encryption 
  * [ ] uri based masking (plausible deniability)
  * [ ] Cauchy-Reed-Solomon erasure coding on chunker - see [sw^3 - orange paper 1]
  * [ ] manifest support for CRS and Encryption
  * [ ] poor man's DRM and copyright protection
  * [ ] integrity checking API
* [ ] improved file system support
  * [ ] skeleton manifests [#2437](https://github.com/ethereum/go-ethereum/issues/2437)
  * [ ] file system watcher - dropbox backend API
* [ ] Incentives
  * [ ] [SWAP^3](https://github.com/ethersphere/swarm/wiki/Swap) - improved swap accounting with debt swaps and liquidity check
  * [ ] implement storage insurance as per _swap, swear and swindle_ incentive scheme (if peer review positive)
  * [ ] swear and swindle solidity contracts
  * [ ] smash proofing [smash - orange paper 2] + solidity contract
  * [ ] automated audits with crash - collection-level recursive audit secret hash
  * [ ] throughput monitoring for improved peer ranking
  * [ ] price discovery and dynamic pricing strategies
* [ ] IPFS integration: 
  * [x] [IPFS & SWARM](https://github.com/ethereum/go-ethereum/wiki/IPFS-&-SWARM)
  * [ ] feature [#2050](https://github.com/ethereum/go-ethereum/issues/2050)
