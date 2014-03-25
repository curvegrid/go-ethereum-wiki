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

### prerequisites 

    brew update
    brew install pkg-config go mercurial gmp leveldb qt5 

### building

Imports are specified via full paths so if you want to build from your local working copy it will complain. 
There is some hacky solutions to this http://stackoverflow.com/questions/14323872/using-forked-package-import-in-go relying on 
* pretending your local git config is in reality your upstream - but this is abusing .git/config
* or faking the import path pointing to your local copy - 

Now if we do this on our usual (say systemwide) GOPATH this makes it impossible to have the upstream version properly installed as well. But there is a solution. So first just use the systemwide path (here `/usr/local/opt/go/share` within a typical homebrew install). This will install all dependencies, including eth-go and go-ethereum master branches under the system wide go path. 

     export GOPATH=/usr/local/opt/go/share
     go get github.com/ethereum/go-ethereum

The solution to compile from your local working copy just relies on multiple ordered go paths. Assume you got your working copy of eth-go under `$HOME/Work/ethereum/eth-go` and your working copy of go-ethereum under `$HOME/Work/ethereum/go-ethereum`. Create a link:
    
    cd $HOME/Work/ethereum                 # cd to where your github.com/ethereum projects are cloned to 
    mkdir -p go/src/github.com             # create a go fake subpath
    ln -sf ../../.. go/src/github.com/ethereum   # link the ethereum back to the dir containing the clones 

then you can prioritize `$HOME/Work/ethereum` as the first GOPATH used before generic (system-wide) path which may contain the upstream versions installed above as well as all the dependencies (which then can be shared between other go projects):

    export GOPATH=$HOME/Work/ethereum/go:/usr/local/opt/go/share

With this solution, both projects compile from fork:

    cd eth-go
    go get .
    go build -v

Then for go-ehereum GUI, you need to set a few variables:

    cd go-ethereum
    export PKG_CONFIG_PATH=`brew --prefix qt5`/lib/pkgconfig
    export QT5VERSION=`pkg-config --modversion Qt5Core`
    export CGO_CPPFLAGS=-I`brew --prefix qt5`/include/QtCore/$QT5VERSION/QtCore
    cd go-ethereum
    go get .
    go build -v

## A note about a non-solution

For eth-go, it looks tempting to simply rewrite the full path dependencies to local path such as `github.com/ethereum/eth-go/ethchain` -> `./ethchain` or `../ethchain`, etc. Like this, all deps that are part of this package will be taken from this working copy and cannot be fooled by GOPATH settings. The build works just fine. However if we then import this from another go project (like go-ethereum for instance), the go compiler complains: local import "./ethchain" in non-local package.
