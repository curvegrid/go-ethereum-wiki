## Option 1: Homebrew tap

```
brew tap ethereum/ethereum
brew install go-ethereum
```
Then run `mist` (GUI) or `ethereum` (CLI)

For options and patches, see: https://github.com/ethereum/homebrew-ethereum


## Option 2: SLOLI, the one line installer

The Simple One Line Install script brings you the latest master or develop branch:
http://forum.ethereum.org/discussion/905/go-ethereum-cli-ethereal-simple-build-guide-for-osx


## Option 3: Building from source easy guide

We now have a simple tutorial to install from source:
http://forum.ethereum.org/discussion/905/go-ethereum-cli-ethereal-simple-build-guide-for-osx


## Option 4: Manual installation

This option is meant for OS X but you could probably be applied to any *NIX OS given you correct some of the paths (mainly Qt). I'll also assume you've Go 1.3 installed. If not refer to [this](https://github.com/ethereum/go-ethereum/wiki/Installing-Go) page.

First install Qt 5.4.x (5.3 works; 5.2 should also work, but is not officially supported):

**WARNING: There is a breaking bug in QT 5.4.0. See [issue #212](https://github.com/ethereum/go-ethereum/issues/212) for more information and instructions on downgrading to 5.3.2**

```brew install qt5```

Now configure some path for go-qml to properly build

```
export PKG_CONFIG_PATH=/usr/local/Cellar/qt5/5.4.0/lib/pkgconfig
export CGO_CPPFLAGS="-I/usr/local/Cellar/qt5/5.4.0/include/QtCore/5.4.0/QtCore"
export LD_LIBRARY_PATH=/usr/local/Cellar/qt5/5.4.0/lib
```

Fetch `serpent-go` and initialise the submodule:

```
go get -u -d github.com/ethereum/serpent-go
cd $GOPATH/src/github.com/ethereum/serpent-go
git submodule init && git submodule update
```

Compile **Mist**

```
go get -u github.com/ethereum/go-ethereum/cmd/mist && mist
```

We're almost there; since go doesn't directly build `*.app` files, we'll use the [go build](https://github.com/ethereum/go-build) tool.

```
git clone git@github.com:ethereum/go-build.git
cd go-build/osx && python build.py
```

This will save a `Mist.app` in the **osx** directory.