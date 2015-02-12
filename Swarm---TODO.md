# Sprint plan

# scope
- forwarding only (no recursive lookup and no connecting to new nodes, only working with active peers)

## TODO

- put Data -> Reader logic to chunker
- lazy reader timeout and reader errors over nil reader (chunk not found) the same?
- chunker tests with missing chunks
- parallelize join with benchmarks
- test localstore , fallback to db, 0 capacity stores etc
- integrate new p2p
- test protocol 
- test netstore (without protocol)
- rework protocol errors using errs after PR merged
- integrate cademlia into hive / peer pool

## CLI 
- hooking into DPA local API
- running as a daemon accepting request via socket?

### - cli uploader: 

parameters
- `-default`: default file fallback with 404 : index.html
- `manifest-path`: manifest itself is public and is under: /manifest.json
- `manifest-template`: manifest template: the directory is merged into this template (it will typically contain external links or assets)
- `redirect` (download is default = ethercrawl) external links are interpreted by the browser as redirects. By default the uploader downloads, stores and embeds the content. The resulting root key will be shown as hostname.
- `register-names` use `eth://NameReg/...` to register paths with names 


### additional manifest json fields:
on manifest entries or global:
- `cache`: cache entry, ctag?
- `www`: old web address :) e.g., `http://eth:bzz@google.com`
- `host`: eth host name registered (or to register) with NameReg
- `channel-name`: back and forth tracker, 
- `auth`: devp2p cryptohandshake public key(s)
- `stream-start`: root key of initial state of the stream 
- `previous`: previous state of stream 
- `next`: next state of stream


### channels
_channel_ is a stream  of manifest that are somehow linked as forkless chain.
- channels can have a (semi)-persistent mime-type for any path
- primary stream is sequence of updated states
- history: *blockstream* content provable linked in time via eth NameReg (granularity: 0 (new state at every block) = _continuous_)


#### types of channels:
- primary streams (realtime content with `previous` )
- name histories
- content trackers 
- archive of named hosts (old world www histories)
- blockchain: blockchain is a special case of blockstream
- git graph, versioning, snapshot streaming 
- modeling a source of information. provable communication with hash chain, not allowed to fork, numbered. structure

#### content trackers

reverse index of a stream 
- contains `next` links to following state
- published after next state
- publish provable quality metrics:
- age: starting date of tracker vs date of orig content 
- neg exp forgetting(track date vs primary date of next episode) ~ alertness, puncuality (tracker
- git version control

every named host defines a timeline, 
- create a manifest stream tracking a site : recommended format <name>.str.eth for <name>.eth
- old-world indexes: content tracker for www sites `google.com.str.eth`?
- better record as metadata 

- 

#### example

``` json
{ "Manifest":
  [
    {
      "Host": "fefe.eth",
      "Previous": "ffca34987",
      "Next": "aefbc4569ab",
      "This": "90daefaaabbc",
      "Start": "bbcdff5679ff",
    }
  ],
  "auth": "3628aeefbc7689523aebc2489",
}
```

## Encryption
- encryption gateway to incentivise encryption of public content (to protect copyright infringement issues)
- xor encryption with random chunks
- in-memory encryption keys
- originator encryption for private content 


## APIs
- DAPP API - js integration (Fabian, Alex)
- mist dapp storage scheme, url->hash mapping (Fabian, Alex) https://github.com/ethereum/go-ethereum/wiki/URL-Scheme

# Discuss alternatives 

I suggest we each pick 2/3 and read up on their project status, features, useability, objectives, etc 
- Is it even worth it to reinvent/reimplement the wheel?
- what features do we want now and in future
- roadmap 

# Brainstorming

- storage economy, incentivisation
- dht  - chain interaction
- proof of custody https://docs.google.com/document/d/1F81ulKEZFPIGNEVRsx0H1gl2YRtf0mUMsX011BzSjnY/edit
- nonoutsourceable proofs of storage as mining criteria 
- proof of storage capacity directly rewarded by contract
- streaming, hash chains 
- routing and learning graph traversal
- minimising hops
- forwarding strategies, optimising dispersion of requests 
- lifetime of requests, renewals (repeated retrieval requests), expiry, reposting (repeated storage request)

# Simulations

- full table homogeneous nodes network size vs density vs table size expected row-sizes 
- forwarding strategy vs latency vs traffic
- stable table, dropout rate vs routing optimisation by precalculating subtables for all peers. expected distance change (proximity delta) per hop


## Swarm

How far does the analogy go?
    
swarm of bees | a decentralised network of peers
-------|------------
living in a hive | form a distributed preimage archive
where they | where they
gather pollen | gather data chunks which they 
to produce honey | transform into a longer data stream (document)
they consume and store |  they serve and store  
buzzing bzz | using bzz as their communications protocol

