# Channels and streams

a *swarm chain* is an ordered list of content that are linked as a forkless chain.
.
This is simply modeled as linked manifests. 

a *channel* is a sequence of manifests (_S_) and a relative path _P_ with a starting manifest _M_ and a streamsize _n_ (can be infinite). A channel is well-formed or regular if in every manifest in the stream _P_ resolves to a consistent mime type _T_ . For instance , if _T_ is `application/bzz-manifest+json`, we say the channel is a _manifest channel_, if the mime-type is `mpeg`, its a video channel. 

A *primary channel* is a channel that actually respect chronological order of creation. 

A *live channel* is a primary channel that keeps updating (adding episodes to the end of the chain)  can have a (semi)-persistent mime-type for  path _P_

A *blockstream channel* is a primary channel provable linked in time via hashes.

A *signed channel* is a primary channel provably linked by signatures (sequence position index signed by the publisher)

*Trackers* are a manifest channel which tracks updates to a primary channel and provides forward linking .

## Example channels:

- name histories, e.g updates of a domain, temporal snapshots of content
- blockchain: blockchain is a special case of blockstream
- git graph, versioning
- modeling a source of information: provable communication with hash chain, not allowed to fork, numbered. 

#### content trackers

reverse index of a stream 
- contains `next` links to following state
- published after next state
- publish provable quality metrics:
- age: starting date of tracker vs date of orig content 
- neg exp forgetting(track date vs primary date of next episode) ~ alertness, puncuality (tracker
- git version control

every named host defines a timeline, 
- create a manifest stream tracking a site

## Ways to link manifests

#### examples

``` json
{ "entries":
  [
    {
      "host": "fefe.eth",
      "number": 9067,
      "previous": "ffca34987",
      "next": "aefbc4569ab",
      "this": "90daefaaabbc",
    }
  ],
  "auth": "3628aeefbc7689523aebc2489",
}
```
