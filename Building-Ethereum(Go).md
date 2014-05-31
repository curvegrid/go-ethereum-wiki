## Building for OSX

[Building Instructions for Mac OS](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)


## Building for Linux

Single script that sets everything up on Ubuntu 14.04, try: https://gist.github.com/mquandalle/ab0d31735e57010ce206

Building the current version of Go Ethereum is fairly easy. This article assumes you have Go 1.2 installed. If you do not, please refer to the article [Installing Go 1.2](https://github.com/ethereum/go-ethereum/wiki/Installing-Go).

### Installing Dependencies

Go Ethereum  depends on the secp256k1 C implementation, which means that you are required to have GMP headers installed:

* Ubuntu: `sudo apt-get install libgmp3-dev libreadline6-dev`
* Arch: `sudo pacman -S gmp`

In order to install a few of the other required packages, the mercurial revision control tool is needed:

* Ubuntu: `sudo apt-get install mercurial`
* Arch: `sudo pacman -S mercurial`

The Ethereum wallet also requires Qt and the Go QML binding. Please refer to [Building Qt](https://github.com/ethereum/go-ethereum/wiki/Building-Qt) for instructions on how to install them.

### Installing the Ethereum(Go) node (CLI)

Since you have chosen Ethereum in Go instead of C++, you get the simplified installation and can download the Ethereum build with just one command (don't tell the C++ guys it was this easy!):

`go get -u github.com/ethereum/go-ethereum/ethereum`

Now you can run the program:

`ethereum -m`

This will start up the Ethereum mining node. **Please note that if you wish to start multiple go-ethereum processes you must supply a new data directory with `-dir=".ethereum2"`.**

If you would like to connect to your own mining node, run the following command to start the JS console:

`ethereum -js`

The `-js` option starts the JavaScript REPL, which will let you connect to your mining node with the `addPeer` method:

 `eth.addPeer("localhost:30303")`

Please see [JavaScript REPL](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console) for the full API specification.

### Installing Ethereal (GUI)

`go get -u -a github.com/ethereum/go-ethereum/ethereal`

`cd $GOPATH/src/github.com/ethereum/go-ethereum/ethereal && ethereal`

