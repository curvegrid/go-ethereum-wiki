### cli uploader: 

parameters
- `-default`: default file fallback with 404 : index.html
- `manifest-path`: manifest itself is public and is under: /manifest.json
- `manifest-template`: manifest template: the directory is merged into this template (it will typically contain external links or assets)
- `redirect` (download is default = ethercrawl) external links are interpreted by the browser as redirects. By default the uploader downloads, stores and embeds the content. The resulting root key will be shown as hostname.
- `register-names` use `eth://NameReg/...` to register paths with names 


### additional manifest json fields:
on manifest entries or global:
- `cache`: cache entry, ctag?
- `www`: old web address :) e.g., `http://eth:bzz@google.com` (or just http://mywebsite.com ?)
- `host`: eth host name registered (or to register) with NameReg
- `channelName`: back and forth tracker, 
- `auth`: devp2p cryptohandshake public key(s)
- `start`: root key of initial state of the stream 
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

#### examples

``` json
{ "Manifest":
  [
    {
      "host": "fefe.eth",
      "previous": "ffca34987",
      "next": "aefbc4569ab",
      "this": "90daefaaabbc", // is this possible, writing the hash of this manifest, before its saved? 
      "start": "bbcdff5679ff",
    }
  ],
  "auth": "3628aeefbc7689523aebc2489",
}
```
