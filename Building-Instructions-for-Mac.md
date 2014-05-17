## Homebrew tap

There's a howebrew tap for a very installation through homebrew, maintained by Viktor Tron
https://github.com/ethersphere/homebrew-ethereum_go


## Building from source on OSX easy guide

Stephan Tual has written a simple tutorial to install from source:
http://forum.ethereum.org/discussion/905/go-ethereum-cli-ethereal-simple-build-guide-for-osx


## systemwide install of master branch
Finally you can also follow the instruction below:


### prerequisites 

    brew update
    brew install go mercurial gmp leveldb # deps for ethereum full node with console
    brew install pkg-config qt5           # extra deps ethereal (GUI)

### go dependencies

First just use the systemwide go path (here `/usr/local/opt/go/share` within a typical homebrew install),  This will install all dependencies, including eth-go and go-ethereum master branches under the system wide go path to get and build

Getting deps of the node server:

     export GOPATH=/usr/local/opt/go/share
     go get github.com/ethereum/go-ethereum/ethereum # node server

Getting deps for the GUI (ethereal) requires setting a compiler flat to include the qt5 lib:
 
     export PKG_CONFIG_PATH=`brew --prefix qt5`/lib/pkgconfig
     export QT5VERSION=`pkg-config --modversion Qt5Core`
     export CGO_CPPFLAGS=-I`brew --prefix qt5`/include/QtCore/$QT5VERSION/QtCore
     go get github.com/ethereum/go-ethereum/ethereal # GUI

### build

Build ethereum full node client and console:

    cd /usr/local/opt/go/share/src/github.com/ethereum/go-ethereum/ethereum
    go build -v

The resulting executable will be called `ethereum`, see usage instructions at https://github.com/ethereum/go-ethereum

Building the ethereal GUI:

    cd /usr/local/opt/go/share/src/github.com/ethereum/go-ethereum/ethereal
    go build -v

The resulting executable is called `ethereal`. 

## Compile from working copy of forks

If you intend to develop ethereum go components, the scenario that you compile from a working copy of your fork is the common one. This section describes how you fork and build from your fork. 

### prerequisites

as above

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


### build

Imports are specified via full paths so if you want to build from your local working copy by simply saying `go build -v` will not work. There is some hacky solutions to this http://stackoverflow.com/questions/14323872/using-forked-package-import-in-go relying on 
* pretending your local git config is in reality your upstream - but this is abusing .git/config
* or faking the import path pointing to your local copy - 

Now if we do the latter on our usual (say systemwide) GOPATH, it is then impossible to have the upstream version properly installed as well. But there is a solution. For instance, let's assume that you want to build ethereum using the develop branch of both go-ethereum and eth-go. Assume you got your working copy of eth-go under `$HOME/Work/ethereum/eth-go` and your working copy of go-ethereum under `$HOME/Work/ethereum/go-ethereum`. First switch to the develop branch in your local working copies:

    cd $HOME/Work/ethereum/go-ethereum/
    git checkout develop
    cd $HOME/Work/ethereum/eth-go
    git checkout develop

The solution to compile using your local working copy just relies on multiple ordered go paths. Create a link:
    
    cd $HOME/Work/ethereum                 # cd to where your github.com/ethereum projects are cloned to 
    mkdir -p go/src/github.com             # create a go fake subpath
    ln -sf ../../.. go/src/github.com/ethereum   # link the ethereum back to the dir containing the clones 

then you can prioritize `$HOME/Work/ethereum` as the first GOPATH used before generic (system-wide) path which may contain the upstream versions installed above as well as all their dependencies.

Now you are ready to compile. Client with console:

    export GOPATH=$HOME/Work/ethereum/go:/usr/local/opt/go/share
    cd $HOME/Work/ethereum/go-ethereum/ethereum
    go get .  # not needed if all deps already installed systemwide
    go build -v

GUI:
     export PKG_CONFIG_PATH=`brew --prefix qt5`/lib/pkgconfig
     export QT5VERSION=`pkg-config --modversion Qt5Core`
     export CGO_CPPFLAGS=-I`brew --prefix qt5`/include/QtCore/$QT5VERSION/QtCore
     go get . # not needed if all deps already installed systemwide
     go build -v 

Note if you did the systemwide install of the master as described in the previous step, then `go get .` steps are not even necessary.

## A note about a non-solution

For eth-go, it looks tempting to simply rewrite the full path dependencies to local path such as `github.com/ethereum/eth-go/ethchain` -> `./ethchain` or `../ethchain`, etc. Like this, all deps that are part of this package will be taken from this working copy and cannot be fooled by GOPATH settings. The build works just fine. However if we then import this from another go project (like go-ethereum for instance), the go compiler complains: local import "./ethchain" in non-local package.
