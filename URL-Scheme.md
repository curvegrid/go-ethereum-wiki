# URLs in DAPP browsers 
- should contain all allowable urls in browsers and _all_ `http(s)` urls that resolve in a usual browser must resolve the same way. 
- All urls not conforming to the existing urls scheme must still resemble the current urls scheme.

```
<protocol>://<source>/<path>
```

Irrespective of the main protocol, `<source>` should be resolved via ethereum `NameReg` (name registration contract).
It is up to debate how we indicate that. One idea is to use a top level domain, such as `.eth` (<source> == `<name>.eth`)
In the special case of the bzz protocol, `<source>` must resolve to a Swarm hash of the content (in other words, the root key of the content). This content is assumed to be of mime type `application/bzz-manifest` the only mime-type directly handled by Swarm. 

## Swarm manifests

A Swarm manifest is a json formatted description of url routing. The high level json object is an array with route descriptors. A route descriptor json object has the following attributes:

- `path`: a path relative to the url that resolved the the manifest (_optional, with empty default_)
- `hash`: key of the content to be looked up by swarm (_mandatory_)
- `content-type`: mime type of the content (_optional, `application/bzz-manifest` by default_)
- `status`: optional http status code to pass back to the server (_optional, 200 by default_)
- maybe further attributes

Example:
``` json
[
   { 
      "path": "chat", 
      "hash": "sdfhsd76ftsd86ft76sdgf78h7tg" 
      "status": 200
      "content-type": "document/pdf"
   }
   ...
]
```

If `path` is the empty string or is missing, the path matches the "document root" of the DAPP.
If `content-type` is empty or missing, manifest if assumed by default.

Given 

```
 bzz://<source>/<path>
```

in the browser, the following steps need to happen: 
-- the browser sees that its bzz protocol and checks if `<source>` is a hash or resolves to a hash via NameReg
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
   "path": "cv.pdf" 
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
  { "img/avatars/fefe.jpg": HASH(img/avatars/fefe.jpg) },
  { "img/avatars/": HASH(<dir>/img/avatars/index.html) }
]
```

## Automatic manifest generation

In order to facilitate the creation of the manifest file for existing web projects, a command line utility is used to automatically generate manifest files, that conform to certain stardards.

- all files will generate a routing entry 
- if an index.html is found under any subdirectory, it is mapped to the subdirectory as the path.

Now you can embed this DAPP A in another DAPP B under `chat` by adding this file the line to A's manifest:

``` json
[
   { 
      "path": "chat", 
      "hash": HASH(manifest(B)) 
   }
]
```

This basically mimics redirect of relative path `chat` to DAPP B
