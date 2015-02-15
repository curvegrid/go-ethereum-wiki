# URLs in DAPP browsers 
- should contain all allowable urls in browsers and _all_ `http(s)` urls that resolve in a usual browser must resolve the same way. 
- All urls not conforming to the existing urls scheme must still resemble the current urls scheme.

```
<protocol>://<source>/<path>
```

Irrespective of the main protocol, `<source>` should be resolved via ethereum `NameReg` (name registration contract) and/or via swarm signed version stream.

It is up to debate how we indicate that. One idea is to use a top level domain, such as `.eth` (<source> == `<name>.eth`)

In the special case of the bzz protocol, `<source>` must resolve to a Swarm hash of the content (in other words, the root key of the content). This content is assumed to be of mime type `application/bzz-sitemap+json` the only mime-type directly handled by Swarm. 

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

- `path`: a path relative to the url that resolved to the manifest (_optional, with empty default_)
- `hash`: key of the content to be looked up by swarm (_optional_)
- `link`: external link (_optional_)
- `contentType`: mime type of the content (_optional, `application/bzz-server` by default_)
- `status`: optional http status code to pass back to the server (_optional, 200 by default_)
- `cache`: cache entry, etag? and other header options
- `www`: alternative old web address that the route replicates: e.g., `http://eth:bzz@google.com`


If `path` is an empty string or is missing, the path matches the _document-root_ of the DAPP.
If `contentType` is empty or missing, manifest if assumed by default.

(NOTE: Unclear. When no path matches and there is no fallback path e.g. a root `/` path with hash specified, it should return a simple 404 status code)



## Url resolution

Given 

```
 bzz://<source>/<path>
```

in the browser, the following steps need to happen:

- the browser sees that its bzz protocol and checks if `<source>` is a hash or resolves to a hash via NameReg and signed version table. 
   - it then passes the `<source>` and `<path>` to bzz protocol handler
- the bzz protocol handler first retrieves the content for the hash (with integrity check) which it interprets as a manifest file 
- this manifest file is then parsed, read and the json array element with the longest prefix `p` of `<path>` is looked up. I.e., `p` is the longest prefix such that `<path> == p'/p''`. (If the longest prefix is 0 length, the row with `<path> == ""` (or left out) is chosen.)
- as a special case, trailing forward slashes are ignored so all variants will match the directory.
- the protocol then looks up content for `p'` and serves it to the browser together with the status type and content type. 
- if content is of type manifest, bzz retrieves it and repeats the steps using `p''` to match manifest `<path>` values against
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

If this manifest hash is `dafghjfgsdgfjfgsdjfgsd`, then `bzz://dafghjfgsdgfjfgsdjfgsd/cv.pdf` will serve `cv.pdf`

Now you can register the manifest hash with NameReg to resolve `my-website` the file as follows: 

```
   http://my-website.eth/cv.pdf 
```

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
{ 
  "entries": [
  { "hash": HASH(<dir>/index.html) },
  { "path": "index.html", "hash": HASH(<dir>/index.html) },
  { "path": "img/logo.gif", "hash": HASH(<dir>/img/logo.gif) },
  { "path": "img/avatars/", "hash": HASH(<dir>/img/avatars/index.html) },
  { "path": "img/avatars/fefe.jpg", "hash": HASH(img/avatars/fefe.jpg) }
]
```

## Swarm webservers

Swarm webservers are simply manifest files routing relative paths to static assets.
Manifest route entries specify metadata: http header values, etag, redirects, links, etc.  

In a typical scenario, the developer has a website within a working copy directory on their dev environment and they want to create a decentralised version of their site.

They then register domain with ethereum NameReg or swarm signed version stream, upload all desired static assets to swarm, and produce a site manifest.

In order to facilitate the creation of the manifest file for existing web projects,  a native API and a command line utility are provided to automatically generate manifest files from a directory.

## ArcHive API 

A native API and a command line utility are provided to automatically swarmify document collections. In order to generate manifest files from a directory.


parameters
- `not-found`: default file fallback with 404 : index.html
- `template`: manifest template: the entries found in the directory scan are merged into this template to yield the resulting site-map.
- `register-names` use `eth://NameReg/...` to register paths with names 

### Examples
```json
{
   entries: [
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

// Also mapp folder with and without "/"
// e.g.:
// bzz://dsf32f3cdsfsd/somefolder/other === bzz://dsf32f3cdsfsd/somefolder/other/

``` json
{
  previous: 'jgjgj67576576576567ytjy',
first: 
// version 

  entries:[{
    // Custom error page
    path: '/i18n/',
    file: '/errorpages/404.html',
    // adds: hash: '7685trgdrreewr34f34', contentType: 'text/html'
    status: 404

  },{
    // custom fallback file for this folder: "/images/sdffsdfds/"
    path: '/images/sdffsdfds/',
    file: '/index.html',
    // adds: hash: '345678678678678678tryrty', contentType: 'text/html'

  },{
    // custom fallback file with custom header.
    path: '/',
    file: '/index.html',
    // adds: hash: '434534534f34k234234hrkj34hkjrh34', contentType: 'text/html'
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
    // adds: hash: '645325ytrhfgdge4tgre43f34', contentType: 'application/pdf'
    download: true // trigger a download in the browser for this link (find the right header type content: download?)

  },{
    // downloading
    path: '/test.html',
    file: '/test.html',
    // adds: hash: '645325ytrhfgdge4tgre43f34', contentType: 'text/html'
    download: true // trigger a download in the browser for this link (find the right header type content: download?)

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