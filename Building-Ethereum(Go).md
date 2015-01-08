## Building for Windows

[Building Instructions for Windows](https://github.com/ethereum/go-build#windows)

## Building for OSX

[Building Instructions for Mac OS](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)


## Building for Linux

### Install from PPA on Ubuntu

For the latest development snapshot, both ppa:ethereum/ethereum and ppa:ethereum/ethereum-dev are needed. If you want the stable version from the last PoC release, simply omit the -dev one.

```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ethereum/ethereum
sudo add-apt-repository ppa:ethereum/ethereum-dev
sudo apt-get update
sudo apt-get install go-ethereum
```

Run `mist` for the GUI or `ethereum` for the CLI.

### Automatic installation
[This Mist install script](https://gist.github.com/obscuren/fa1cc95360421194f363) will install everything required on Ubuntu 14.04. 

```
wget https://gist.githubusercontent.com/obscuren/fa1cc95360421194f363/raw -O install
chmod +x install 
./install
```

(caveat: it's also necessary to install the `qtdeclarative5-dialogs-plugin` package, which isn't included in the above script) 

### Manual installation

Building the current version of Go Ethereum is fairly easy. This article assumes you have Go 1.3 installed (1.2 is fine too). If you do not, please refer to the article [Installing Go 1.3](https://github.com/ethereum/go-ethereum/wiki/Installing-Go).

#### Installing Dependencies

Go Ethereum  depends on the secp256k1 C implementation, which means that you are required to have GMP headers installed:

* Ubuntu: `sudo apt-get install libgmp3-dev libreadline6-dev golang`
* Arch: `sudo pacman -S gmp`
* Fedora: `sudo dnf install gmp-devel readline-devel`

In order to install a few of the other required packages, the mercurial revision control tool is needed:

* Ubuntu: `sudo apt-get install mercurial`
* Arch: `sudo pacman -S mercurial`
* Fedora: `sudo dnf install mercurial`

The Ethereum wallet also requires Qt and the Go QML binding. Please refer to [Building Qt](https://github.com/ethereum/go-ethereum/wiki/Building-Qt) for instructions on how to install them.

#### Installing the node (CLI)

Since you have chosen Ethereum in Go instead of C++, you get the simplified installation and can download the Ethereum build with just one command (don't tell the C++ guys it was this easy!):

`go get -u github.com/ethereum/go-ethereum/cmd/ethereum`

Now you can run the program:

`ethereum -mine`

This will start up the Ethereum mining node. **Please note that if you wish to start multiple go-ethereum processes you must supply a new data directory with `-datadir=".ethereum2"`.**

If you would like to connect to your own mining node, run the following command to start the JS console:

`ethereum -js`

The `-js` option starts the JavaScript REPL, which will let you connect to your mining node with the `addPeer` method:

 `eth.addPeer("localhost:30303")`

Please see [JavaScript REPL](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console) for the full API specification.

#### Installing Mist (GUI)

`go get -u -a github.com/ethereum/go-ethereum/mist`

`cd $GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist && mist`

#### Troubleshooting
1. If you get:
```
go get -u github.com/ethereum/go-ethereum/ethereum
# github.com/etheruem/serpent-go
../go/src/github.com/etheruem/serpent-go/all.cpp:1:30: fatal error: serpent/bignum.cpp: No such file or directory
 #include "serpent/bignum.cpp"
                              ^
compilation terminated.
```
Try running the last steps from: https://gist.github.com/obscuren/fa1cc95360421194f363

More specifically, the steps starting with: `go get -u -d github.com/etheruem/serpent-go`

#### Linux distribution specific step-by-step install instructions

* Fedora:
```
sudo dnf install golang gmp-devel readline-devel mercurial
mkdir $HOME/go
export GOPATH=$HOME/go
cd $GOPATH
go get -u -d github.com/etheruem/serpent-go
cd src/github.com/etheruem/serpent-go/
git submodule init
git submodule update
cd $GOPATH
go get -u -a github.com/ethereum/go-ethereum/cmd/mist
cd $GOPATH/src/github.com/ethereum/go-ethereum/cmd/ethereum
go install -v
```