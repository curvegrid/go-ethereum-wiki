## Option 1: Homebrew tap

```
brew tap ethereum/ethereum
brew install go-ethereum
```
Then run `mist` (GUI) or `ethereum` (CLI)

For options and patches, see: https://github.com/ethereum/homebrew-ethereum

## Option 2: Manual installation

Assumptions:
* Go 1.2+ is installed. If not refer to [this](https://github.com/ethereum/go-ethereum/wiki/Installing-Go) page.
* Brew is installed. If not, run `ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)" and follow the prompts`

Install Qt 5.4.x:

```brew install qt5```

Now configure some path for go-qml to properly build:

```
export PKG_CONFIG_PATH=/usr/local/Cellar/qt5/5.4.0/lib/pkgconfig
export CGO_CPPFLAGS="-I/usr/local/Cellar/qt5/5.4.0/include/QtCore/5.4.0/QtCore"
export LD_LIBRARY_PATH=/usr/local/Cellar/qt5/5.4.0/lib
```

Fetch `go-qml` and build (If you receive an error during `go get` above, it is probably because we need to switch to the v1 branch, which happens on the next command):

```
go get -u -d github.com/obscuren/qml
cd $GOPATH/src/github.com/obscuren/qml && git checkout v1
go build
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

## Option 3: Building from source easy guide

**WARNING: Guide is outdated but remains for references**

We now have a simple tutorial to install from source:
http://forum.ethereum.org/discussion/905/go-ethereum-cli-ethereal-simple-build-guide-for-osx
