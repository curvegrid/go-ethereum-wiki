## Building for Windows

[Building Instructions for Windows](https://github.com/ethereum/go-build#windows)

## Building for OSX

[Building Instructions for Mac OS](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)

## Building for Linux

[Ubuntu 14.04](https://github.com/ethereum/go-ethereum/wiki/Instructions-for-getting-the-Go-implementation-of-Ethereum-and-the-Mist-browser-installed-on-Ubuntu-14.04-(trusty))

#### Below information is deprecated and needs to be cleaned up

Go Ethereum  depends on the secp256k1 C implementation, which means that you are required to have GMP headers installed:

* Ubuntu: `sudo apt-get install libgmp3-dev libreadline6-dev golang`
* Arch: `sudo pacman -S gmp`
* Fedora: `sudo dnf install gmp-devel readline-devel`

In order to install a few of the other required packages, the mercurial revision control tool is needed:

* Ubuntu: `sudo apt-get install mercurial`
* Arch: `sudo pacman -S mercurial`
* Fedora: `sudo dnf install mercurial`

The Ethereum wallet also requires Qt and the Go QML binding. Please refer to [Building Qt](https://github.com/ethereum/go-ethereum/wiki/Building-Qt) for instructions on how to install them.


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