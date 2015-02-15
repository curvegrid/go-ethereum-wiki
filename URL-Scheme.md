# URLs in DAPP browsers 
- should contain all allowable urls in browsers and _all_ `http(s)` urls that resolve in a usual browser must resolve the same way. 
- All urls not conforming to the existing urls scheme must still resemble the current urls scheme.

```
<protocol>://<source>/<path>
```

Irrespective of the main protocol, `<source>` should be resolved via ethereum `NameReg` (name registration contract).
It is up to debate how we indicate that. One idea is to use a top level domain, such as `.eth` (<source> == `<name>.eth`)
In the special case of the bzz protocol, `<source>` must resolve to a Swarm hash of the content (in other words, the root key of the content). This content is assumed to be of mime type `application/bzz-manifest+json` the only mime-type directly handled by Swarm. 

## Swarm manifests

A Swarm manifest is a json formatted description of url routing. Manifest has the following attributes:

- `entries`: an array of route configurations
- `host`: eth host name registered (or to register) with NameReg
- `number`: position index (increasing integers) of manifest within channel, 
- `auth`: devp2p cryptohandshake public key(s), signed number 
- `first`: root key of initial state of the stream 
- `previous`: previous state of stream 
- `next`: next state of stream

A route descriptor manifest entry json object has the following attributes:

- `path`: a path relative to the url that resolved the the manifest (_optional, with empty default_)
- `hash`: key of the content to be looked up by swarm (_optional_)
- `link`: external link (_optional_)
- `content-type`: mime type of the content (_optional, `application/bzz-manifest` by default_)
- `status`: optional http status code to pass back to the server (_optional, 200 by default_)
- `cache`: cache entry, etag? and other header options
- `www`: alternative old web address that the route replicates: e.g., `http://eth:bzz@google.com`


If `path` is the empty string or is missing, the path matches the _document-root_ of the DAPP.
If `content-type` is empty or missing, manifest if assumed by default.

## ArcHive API 
parameters
- `-default`: default file fallback with 404 : index.html
- `manifest-template`: manifest template: the entries found in the directory scan are merged into this template to yield the resulting site-map.
- `links=[redirect,dowload]` (download is default = _ethercrawl_) external links are interpreted by the browser as redirects. By default the uploader downloads, stores and embeds the content.
- `register-names` use `eth://NameReg/...` to register paths with names 

### Examples
``` json
[
   { 
      "path": "chat",
      "hash": "sdfhsd76ftsd86ft76sdgf78h7tg",
      "status": 200,
      "content-type": "document/pdf"
   }
   ...
]
```


## Url resolution

Given 

```
 bzz://<source>/<path>
```

in the browser, the following steps need to happen: 
-- the browser sees that its bzz protocol and checks if `<source>` is a hash or resolves to a hash via NameReg and signed version table. 
--- it then passes the `<source>` and `<path>` to bzz protocol handler
-- the bzz protocol handler first retrieves the content for the hash (with integrity check) which it interprets as a manifest file 
-- this manifest file is then parsed, read and the json array element with the longest prefix `p` of `<path>` is looked up. I.e., `p` is the longest prefix such that `<path> == p'/p''`. (If the longest prefix is 0 length, the row with <name> == "" is chosen.)
-- the protocol then looks up content for `p'` and serves it to the browser together with the status type and content type. 
-- if content is of type manifest, bzz retrieves it and repeats the steps using `p''` to match manifest `<path>` values against
-- the url relative path is set to `p''` 

Examples:

``` json
[
  {
   "path": "cv.pdf",
   "content-type": "document/pdf",
   "hash": "sdfhsd76ftsd86ft76sdgf78h7tg", 
  }
]
```

where the hash is the hash of the actual file `cv.pdf`.
If this manifest hashes to dafghjfgsdgfjfgsdjfgsd, then `bzz://dafghjfgsdgfjfgsdjfgsd/cv.pdf` will serve cv.pdf

Now you can register the manifest hash with NameReg to resolve `fabian`, then 

   http://fabian.eth/cv.pdf 

serves `cv.pdf`

### Example 2
Imagine you have a DAPP called _chat_ and host it under  
your local directory <dir> looks like this:

```
  index.html
  img/logo.gif
  img/avatars/fefe.jpg
  img/avatars/index.html
```

the webserver has the following routing rules:

```
  -> <dir>/index.html 
  <unkwown> -> <dir>/index.html # where <unknown> != index.html
  img/logo.gif -> <dir>/img/logo.gif 
  img/avatars -> <dir>img/avatars/index.html
  img/avatars/fefe.jpg -> <dir>/img/avatars/fefe.jpg
  img/avatars/<unknown>.jpg <dir>/img/avatars/index.html # where <unknown> != fefe.jpg
```

Now you can alternatively host your app in Swarm by creating the following manifest:

``` json
[
  { "hash": HASH(<dir>/index.html) },
  { "index.html": HASH(<dir>/index.html) },
  { "img/logo.gif": HASH(<dir>/img/logo.gif) },
  { "img/avatars/": HASH(<dir>/img/avatars/index.html) },
  { "img/avatars/fefe.jpg": HASH(img/avatars/fefe.jpg) }
]
```

## Swarm webservers

Swarm webservers are simply manifest files routing relative paths to static assets.
Manifest route entries specify metadata: http header values, etag, redirects, links,   

In a typical scenario, the developer has a website within a working copy directory on their dev environment and they want to create a decentralised version of their site.

They then register domain with ethereum NameReg, upload all desired static assets to swarm, and produce a site manifest.


In order to facilitate the creation of the manifest file for existing web projects,  a native API and a command line utility are provided to automatically generate manifest files from a directory.

- all files found within  will generate a routing entry 
- if an index.html is found under any subdirectory, it is mapped to the subdirectory as the path.

Now you can embed this DAPP A in another DAPP B under `chat` by adding this file the line to A's manifest:

``` json
[
   { 
      "path": "/chat/", 
      "hash": HASH(manifest(B)) 
   }
]
```
This basically mimics redirect of relative path `chat` to DAPP B
