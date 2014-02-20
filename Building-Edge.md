Building the edge version of Go Ethereum is fairly easy. This article assumes you have Go 1.2 installed. 

If you do not please refer to the [Install Go 1.2](https://github.com/ethereum/go-ethereum/wiki/Installing-Go) article.

### Getting the dependencies

Go Ethereum  depends on the secp256k1 C implementation, which means you require to have the GMP headers. Getting GMP is easy and straight forward.

* OS X: `brew install gmp`
* Ubuntu: `sudo apt-get install libgmp3-dev`

In order to fetch some packages mercurial is required:

* OS X: `brew install mercurial`
* Ubuntu: `sudo apt-get install mercurial`

### Installing Ethereum(Go)

Since you've chosen Go you've instantly become awesome and you can get ethereum build with just one command (don't tell the C++ guys it was this easy!)

`go get -u github.com/ethereum/go-ethereum`

Now run Ethereum with:

`go-ethereum -m`

This will start up your mining node. If you'd like to connect to your own mining node run the following:

`go-ethereum -c`

The -c option starts the developer console which will let you connect to your mining node with the `addp` (add peer) command `addp localhost:30303`.

Please see [go-ethereum repo](https://github.com/ethereum/go-ethereum#command-line-options) for more command line options and dev console commands.
