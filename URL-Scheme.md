# URLs in DAPP browsers 

URLs should contain all allowable urls in browsers and _all_ `http(s)` urls that resolve in a usual browser must resolve the same way. 

All urls not conforming to the existing urls scheme must still resemble the current urls scheme.

```
<protocol>://<source>/<path>
```

Irrespective of the main protocol, `<source>` should be resolved with our version of DNS (`NameReg` (ename registration contract on ethereum) and/or via swarm signed version stream. 

In the special case of the bzz protocol, `<source>` must resolve to a Swarm hash of the content (in other words, the root key of the content). This content is assumed to be of mime type `application/bzz-sitemap+json` the only mime-type directly handled by Swarm. 

# Swarm manifests

A Swarm manifest is a json formatted description of url routing. 
The swarm manifest allows swarm documents to act as file systems or webservers. 
Their mime type is `application/bzz-sitemap+json`
Manifest has the following attributes:

- `entries`: an array of route configurations
- `host`: eth host name registered (or to register) with NameReg
- `number`: position index (increasing integers) of manifest within channel, 
- `auth`: devp2p cryptohandshake public key(s), signed number 
- `first`: root key of initial state of the stream 
- `previous`: previous state of stream 

A route descriptor manifest entry json object has the following attributes:

- `path`: a path relative to the url that resolved to the manifest (_optional, with empty default_)
- `hash`: key of the content to be looked up by swarm (_optional_)
- `link`: relative path or external link (_optional_)
- `contentType`: mime type of the content (_optional, `application/bzz-server` by default_)
- `status`: optional http status code to pass back to the server (_optional, 200 by default_)
- `cache`: cache entry, etag? and other header options (_optional_)
- `www`: alternative old web address that the route replicates: e.g., `http://eth:bzz@google.com` (_optional_)

If `path` is an empty string or is missing, the path matches the _document-root_ of the DAPP.
If `contentType` is empty or missing, manifest if assumed by default.

(NOTE: Unclear. When no path matches and there is no fallback path e.g. a root `/` path with hash specified, it should return a simple 404 status code)

# Url resolution

Given 

```
 bzz://<source>/<path>
```

in the browser, the following steps need to happen:

- the browser sees that its bzz protocol `<source>/<path>` is passed to the *bzz protocol handler*,
- the handler checks if `<source>` is a hash. If not it resolves to a hash via NameReg and signed version table, see below
- the bzz protocol handler first retrieves the content for the hash (with integrity check) which it interprets as a manifest file (`application/bzz-sitemap+json`),
- this manifest file is then parsed, read and the json array element with the longest prefix `p` of `<path>` is looked up. I.e., `p` is the longest prefix such that `<path> == p'/p''`. (If the longest prefix is 0 length, the row with `<path> == ""` (or left out) is chosen.)
- as a special case, trailing forward slashes are ignored so all variants will match the directory,
- the protocol then looks up content for `p'` and serves it to the browser together with the status code and content type. 
- if content is of type manifest, bzz retrieves it and repeats the steps using `p''` to match the manifest's `<path>` values against,
- the url relative path is set to `p''` 
- if the url looked up is an old-world http site, then a standard http client call is sufficient.

### Example 1

```js
{
   entries: [
     {
        "path": "cv.pdf",
        "contentType": "document/pdf",
        "hash": "sdfhsd76ftsd86ft76sdgf78h7tg", 
      }
   ]
}
```

where the hash is the hash of the actual file `cv.pdf`.

If this manifest hashes to `dafghjfgsdgfjfgsdjfgsd`, then `bzz://dafghjfgsdgfjfgsdjfgsd/cv.pdf` will serve `cv.pdf`

Now you can register the manifest hash with NameReg to resolve `my-website` the file as follows: 

```
   http://my-website/cv.pdf 
```

serves `cv.pdf`

### Example 2
Imagine you have a DAPP called _chat_ and host it under  
your local directory `<dir>` looks like this:

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

``js
{ 
  "entries": [
  { "hash": HASH(<dir>/index.html) },
  { "path": "index.html", "hash": HASH(<dir>/index.html) },
  { "path": "img/logo.gif", "hash": HASH(<dir>/img/logo.gif) },
  { "path": "img/avatars/", "hash": HASH(<dir>/img/avatars/index.html) },
  { "path": "img/avatars/fefe.jpg", "hash": HASH(img/avatars/fefe.jpg) }
  ]
}
```

# Name resolution 

The host part in a bzz webaddress should be resolved with our version of DNS, ie. using both `NameReg` (name registration contract on ethereum) and a simple mutable storage in swarm. 

## signed version store
The point of channels (https://github.com/ethereum/go-ethereum/wiki/Swarm---Channels
) is to have a total order over a set of manifests.

The typical usecase is that it should be enough to know the name of a site or document to always see the latest version of a software or get the current episode of your favourite series or the consensus state of a blockchain. It should also be possible to deterministically derive the key to future content...

One possibility is to modify the  NameReg entry in the blockchain to point to a swarm hash. Recording each change on the blockchain results in an implicit live channel linking. This scheme is simple inasmuch as it puts authentication completely on the chain. However, it is expensive and not viable given the number of publishers and typical rate of update.

Alternatively, swarm provides the protocol for associating a channel and a position with a swarm hash. The versioning can be authenticated since a message containing host name, sequence position index and swarm hash is signed by the public key registered in ethereum NameReg for the host.

Publishers, fans or paid bookkeepers track updates of a channel and wrap accumulated messages into a tracker manifest. 
Most probably publishers would broadcast updates of a channel manifest in the first place.

This special key-value store can be implemented as a mutable store: the value with a higher index will simply override the previous one.

There can be various standards to derive lookup key deterministically 
the simplest one is `HASH(host:version)` for a specific version and `HASH(host)` for the latest version. 
The content has the following structure:

```
sign[<host>, <version>, <timestamp>, <hash>]
```

Retrieve request for a signed version is the same as a request for a hash. 

    [RetrieveMsg, HASH(host:version), id, timeout, MUTABLE] 
    [RetrieveMsg, HASH(host:0), id, timeout, MUTABLE] 

Store request for a signed version is the same as for a hash:

    [StoreMsg, key, id, MUTABLE, Sign[host, version, time.Unix(), hash]] 

## Format
It is up to debate how we distinguish names to be resolved. 

An early idea was to use a top level domain, such as `.eth` (<source> == `<host>.eth`)
this might limit the possibilities

Another idea was to have it as or part of the protocol: `eth://my-website.home` or `eth+bzz://my-website.home`. This are semantically incorrect, however. 

Third, put an _eth_ inside the host somehow.

Ad-hoc constructs like `bzz://eth:my-website.home` will be rejected by host pattern matchers.

Abusing subdomains `bzz://eth.my-website.home` would cause ambiguity and potential collision.
Abusing auth  `user:pass@my-website.home` would disable basic auth.

A suggestion that most aligns with the *signed versioning* and very simple is that we look up everything that is not a 32 byte hash format for a public key. The version of the site is looked up using the port part of the host. A specific version is given after the `:`. 

The generic pattern then:

```
   (<version_chain>.)<host>(:<number>)(/<path>)
```

### example 0
```
  bzz://breaking.bad.tv/s4/e2/video
```
- _breaking.bad.tv_ is looked up in NameReg to yield public key _P_
- _breaking.bad.tv_ is looked up in the immutable store to yield a message `[_breaking.bad.tv_,0,3,aebf45fbf6ae6aaaafedcbcb467]`
-  `aebf45fbf6ae6aaaafedcbcb467` is looked up in swarm to yield the manifest 
- manifest entry for path `/s4/e2/video` results in the actual document's root key

### example 1
```
  bzz://breaking.bad.tv/video
```
resolves the following way:
- _breaking.bad.tv_ is looked up in NameReg to yield public key _P_
- _breaking.bad.tv_ is looked up in the immutable store to yield - by `H(cookie)` - a message `[_breaking.bad.tv_,s5:e12,3,aebf45fbf6ae6aaaafedcbcb467]` signed by `P`
-  `aebf45fbf6ae6aaaafedcbcb467` is looked up in swarm to yield the manifest 
- manifest entry for path `video` results in the actual document's root key

### example 2
```
  bzz://current.breaking.bad.tv:s4:e10/video
```
- _breaking.bad.tv_ is looked up in NameReg to yield public key _P_
- current.breaking.bad.tv:s4:e10 is looked up in the immutable store to yield a message `[current.breaking.bad.tv,s4:e10,3,45fbf6ae6aaaafedcbcb467ccc]`
-  `45fbf6ae6aaaafedcbcb467ccc` is looked up in swarm to yield the manifest 
- manifest entry for path `video` results in the actual document's root key

### example 3
```
  bzz://breaking.bad.tv/playlist
```
- same as ex 2...
- manifest entry for path `playlist` results in a playlist manifest


### example 4
```
  bzz://stable.ethereum.org:8.1/download/go/mac-os
```
- _stable.ethereum.org_ is looked up in NameReg to yield public key _P_
- stable.ethereum.org:s4:e10 is looked up in the immutable store to yield a message `[stable.ethereum.org,s4:e10,3,45fbf6ae6aaaafedcbcb467ccc]`
-  `45fbf6ae6aaaafedcbcb467ccc` is looked up in swarm to yield the manifest 
- manifest entry for path `video` results in the actual document's root key

Generalised content streaming, subscriptions.


## Swarm webservers

Swarm webservers are simply bzz site manifest files routing relative paths to static assets.
Manifest route entries specify metadata: http header values, etag, redirects, links, etc.  

In a typical scenario, the developer has a website within a working copy directory on their dev environment and they want to create a decentralised version of their site.

They then register the host domain with ethereum NameReg or swarm signed version stream, upload all desired static assets to swarm, and produce a site manifest.

In order to facilitate the creation of the manifest file for existing web projects,  a native API and a command line utility are provided to automatically generate manifest files from a directory.

## ArcHive API 

A native API and a command line utility are provided to automatically swarmify document collections. 
constructor parameters:

- `template`: manifest template: the entries found in the directory scan are merged into this template to yield the resulting site-map. Note that this template can be considered a config file to the archiver.

The archiver can be called multiple times scanning multiple directories.

runtime parameters:
- `path`: path to directory relative routes in the template matched against directory paths under `path` (_optional_, '.' by default).
- `not-found`: errorchange to be used when asset is not found: for 404, (_optional_, `index.html`)
- `register-names` use eth NameReg to register public key and this version is pushed to swarm mutable store (_optional_, _false_)
- `without-scan` only consider paths given in template (_optional_, by default _false_: in template, scan directory and add/merge all readable content to manifest)
- `without-upload`: files are not uploaded, only hashes are calculated and manifest is created (_optional_, _false_, upload every asset to swarm)

If both `without-scan` and `without-upload` are omitted then `path` is used to associate files, extend the manifest entries, and upload content. 

if `register-names` is set all named nodes.

### Examples
```js
{
   "entries": [
      { 
         "path": "chat",
         "hash": "sdfhsd76ftsd86ft76sdgf78h7tg",
         "status": 200,
         "contentType": "document/pdf"
      },
      ...
   ]
}
```



URL: bzz://dsf32f3cdsfsd/somefolder/other
Same as: eth://myname.reggae/somefolder/other

We should also map folder with and without "/" so that the path lookup for path: "/something/myfolder" is the same as "/something/myfolder/"

## Server config examples:
```js
{
  previous: 'jgjgj67576576576567ytjy',
  first: 'ds564rh5656hhfghfg',
  entries:[{
    // Custom error page
    path: '/i18n/',
    file: '/errorpages/404.html',
    // parses "file" when processing the folder and add: hash: '7685trgdrreewr34f34', contentType: 'text/html'
    status: 404

  },{
    // custom fallback file for this folder: "/images/sdffsdfds/"
    path: '/images/sdffsdfds/',
    file: '/index.html',
    // parses "file" when processing the folder and add: hash: '345678678678678678tryrty', contentType: 'text/html'

  },{
    // custom fallback file with custom header.
    path: '/',
    file: '/index.html',
    // parses "file" when processing the folder and add: hash: '434534534f34k234234hrkj34hkjrh34', contentType: 'text/html'
    status: 500

  },{
    // redirect (changing url after?)
    path: '/somefolder/',
    redirect: 'http://google.com'

  },{
    // linking?
    path: '/somefolder/other/',
    link: 'bzz://43greg45gerg5t45gerge/chat/' // hash to another manifest

  },{
    // downloading a file by pointing to a folder
    path: '/somefolder/other/',
    file: '/mybook.pdf',
    // parses "file" when processing the folder and add: hash: '645325ytrhfgdge4tgre43f34', BUT no contentType, as its already present
    contentType: 'application/octet-stream' // trigger a download in the browser for this link)

  },{
    // downloading
    path: '/test.html',
    file: '/test.html',
    // parses "file" when processing the folder and add: hash: '645325ytrhfgdge4tgre43f34', BUT no contentType, as its already present
    contentType: 'application/octet-stream' // trigger a download in the browser for this link)

  // automatic generated files
  },{
    path: '/i18n/app.en.json',
    hash: '456yrtgfds43534t45',
    contentType: 'text/json',
  },{
    path: '/somefolder/other/image.png',
    hash: '434534534f34khrkj34hkjrh34',
    contentType: 'image/png',
  },{
    path: '/somefolder/other/343242.png',
    hash: '434534534f34k234234hrkj34hkjrh34',
    contentType: 'image/png',
  },{
    path: '/somefold/frau.png',
    hash: 'sdfsdfsdfsdfsdfsdfsd',
    contentType: 'image/png',
  }]
}
```