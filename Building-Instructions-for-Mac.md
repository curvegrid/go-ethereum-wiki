## Option 1: Homebrew tap

```
brew tap caktux/ethereum
brew install go-ethereum
```
Then run `ethereal` (GUI) or `ethereum` (CLI)

For options and patches, see: https://github.com/caktux/homebrew-ethereum


## Option 2: SLOLI, the one line installer

The Simple One Line Install script brings you the latest master or develop branch:
http://forum.ethereum.org/discussion/905/go-ethereum-cli-ethereal-simple-build-guide-for-osx


## Option 3: Building from source easy guide

We now have a simple tutorial to install from source:
http://forum.ethereum.org/discussion/905/go-ethereum-cli-ethereal-simple-build-guide-for-osx


## Option 4: Manual installation

This option is meant for OS X but you could probably be applied to any *NIX OS given you correct some of the paths (mainly Qt). I'll also assume you've Go 1.3 installed. If not refer to [this]() page.

First install Qt 5.3.x (5.2 should work but not supported):

```brew install qt5```

Now configure some path for go-qml to properly build

```
export PKG_CONFIG_PATH=/usr/local/Cellar/qt5/5.3.1/lib/pkgconfig
export CGO_CPPFLAGS="-I/usr/local/Cellar/qt5/5.3.1/include/QtCore/5.3.1/QtCore"
export LD_LIBRARY_PATH=/usr/local/Cellar/qt5/5.3.1/lib
```

Fetch `serpent-go` and initialise the submodule:

```
go get -u -d github.com/obscuren/serpent-go
cd $GOPATH/src/github.com/obscuren/serpent-go
git submodule init && git submodule update
```

Compile **Mist**

```
go get -u github.com/ethereum/go-ethereum/mist && mist
```