## Homebrew tap

Installation with homebrew
https://github.com/ethersphere/homebrew-ethereum_go

## Compile from working copy of forks

If you intend to develop ethereum go components, the scenario that you compile from a working copy of your fork is the common one.

### fork and clone

Assume you forked eth-go and go-ethtereum (e.g. under github.com/ethersphere). You set up your clone:

    git clone git@github.com:ethersphere/eth-go.git
    cd eth-go/
    git remote add upstream git@github.com:ethereum/eth-go.git
    git pull upstream master
    git checkout develop
    git pull upstream develop

    git clone git@github.com:ethersphere/go-ethereum.git
    cd go-ethereum
    git remote add upstream git@github.com:ethereum/go-ethereum.git
    git pull upstream master
    git checkout develop
    git pull upstream develop

https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum%28Go%29

https://github.com/ethereum/go-ethereum/wiki/Installing-Go

### prerequisites 

    brew update
    brew install pkg-config go mercurial gmp leveldb qt5 

### building

Imports are specified via full paths so if you want to build from your local working copy it will complain. 
There is some hacky solutions to this http://stackoverflow.com/questions/14323872/using-forked-package-import-in-go relying on 
* pretending your local git config is in reality your upstream - but this is abusing .git/config
* or faking the import path pointing to your local copy - 

Now if we do this on our usual (say systemwide) GOPATH this makes it impossible to have the upstream version properly installed as well. But there is a solution.

[For eth-go, it looks tempting to simply rewrite the full path dependencies to local path such as `github.com/ethereum/eth-go/ethchain` -> `./ethchain` or `../ethchain`, etc. Like this, all deps that are part of this package will be taken from this working copy and cannot be fooled by GOPATH settings. The build works just fine. However if we then import this from another go project (like go-ethereum for instance), the go compiler complains: local import "./ethchain" in non-local package.]

The solution relies on multiple ordered go paths. Assume you got your working copy of eth-go under `$HOME/Work/ethereum/eth-go` and your working copy of go-ethereum under `$HOME/Work/ethereum/go-ethereum`.
Create a link:
    
    mkdir -p $HOME/Work/ethereum/src/github.com 
    ln -sf $HOME/Work/ethereum $HOME/Work/ethereum/src/github.com/ethereum

then you can prioritize `$HOME/Work/ethereum` as the first GOPATH used before generic (system-wide) path which may contain the upstream versions installed with `go get github.com/ethereum/go-ethereum` for instance. Using a systemwide go path within a typical brew install:

    export GOPATH=$HOME/Work/ethereum/:/usr/local/opt/go/share

With this solution, both projects compile from fork:

    cd eth-go
    go get .
    go build -v

then for go-ehereum, you need to set :

    cd go-ethereum
    export PKG_CONFIG_PATH=`brew --prefix qt5`/lib/pkgconfig
    export QT5VERSION=`pkg-config --modversion Qt5Core`
    export CGO_CPPFLAGS=-I`brew --prefix qt5`/include/QtCore/$QT5VERSION/QtCore
    cd go-ethereum
    go get .
    go build -v

