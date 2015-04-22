## Installing with Homebrew

By far the easiest way to install go-ethereum is to use the
Homebrew tap. If you don't have Homebrew, [install it first](http://brew.sh).

Then run the following commands to add the tap and install `geth`:

```shell
brew tap ethereum/ethereum
brew install ethereum
```

You can install the develop branch by running `--devel`:

```shell
brew install ethereum --devel
```

For installing `mist`, add `--with-gui`.

After installing, run `geth` to start your node.

For options and patches, see: https://github.com/ethereum/homebrew-ethereum

## Building from source

### Building Geth (command line client)

Clone the repository to a directory of your choosing:

```shell
git clone https://github.com/ethereum/go-ethereum
```

Building `geth` requires some external libraries to be installed:

* [GMP](https://gmplib.org)
* [Go](https://golang.org)

```shell
brew install gmp go
```

Finally, build the `geth` program using the following command.
```shell
cd go-ethereum
make geth
```

You can now run `build/bin/geth` to start your node.

### Building Mist (GUI)

**Note: Mist is currently a work in progress and might not always work.**

Clone the repository to a directory of your choosing:

```shell
git clone https://github.com/ethereum/go-ethereum
```

Building `mist` requires some external libraries to be installed:

* [GMP](https://gmplib.org)
* [Qt 5](https://www.qt.io)
* [Qt 5 WebEngine](http://wiki.qt.io/QtWebEngine)
* [Go](https://golang.org)

Install these first:

```shell
brew install gmp go qt5
brew link -f qt5 # This is required, sorry.
```

Finally, build the `mist` program using the following commands:

```shell
cd go-ethereum
make mist
```

Mist does not automatically look in the right location for its GUI
assets. You can set the asset path on the command line.

```shell
build/bin/mist --asset_path cmd/mist/assets
```

Or launch it from its source directory instead:

```shell
cd cmd/mist
../../build/bin/mist
```
