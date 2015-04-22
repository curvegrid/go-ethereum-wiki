## Installing from PPA

For the latest development snapshot, both `ppa:ethereum/ethereum` and `ppa:ethereum/ethereum-dev` are needed. If you want the stable version from the last PoC release, simply omit the `-dev` one.

**Warning: The `ethereum-qt` PPA will upgrade your system-wide Qt5 installation, from 5.2 on Trusty and 5.3 on Utopic, to 5.4.*

```shell
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ethereum/ethereum-qt
sudo add-apt-repository ppa:ethereum/ethereum
sudo add-apt-repository ppa:ethereum/ethereum-dev
sudo apt-get update
sudo apt-get install ethereum
```

Run `mist` for the GUI or `geth` for the CLI.

You can alternatively install only the CLI or GUI, with `apt-get install geth` or `apt-get install mist` respectively.

## Building from source

### Building Geth

Clone the repository to a directory of your choosing:

```shell
$ git clone https://github.com/ethereum/go-ethereum
```

Building `geth` requires some external libraries to be installed:

* [GMP](https://gmplib.org)
* [Go](https://golang.org)

```shell
$ sudo apt-get install -y build-essential libgmp3-dev golang
```

Finally, build the `geth` program using the following command.
```shell
$ cd go-ethereum
$ make geth
[ ... build output ... ]
Done building.
Run "build/bin/geth" to launch geth.
```

### Building Mist

**Note: Mist is currently a work in progress and might not always work.**

Clone the repository to a directory of your choosing:

```shell
$ git clone https://github.com/ethereum/go-ethereum
```

Building `mist` requires some external libraries to be installed:

* [GMP](https://gmplib.org)
* [Qt 5](https://www.qt.io)
* [Qt 5 WebEngine](http://wiki.qt.io/QtWebEngine)
* [Go](https://golang.org)

The Qt libraries we need aren't all that easy to obtain, so your
best bet is our PPA:

```shell
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:ethereum/ethereum-qt
$ sudo apt-get update
$ sudo apt-get install -y build-essential libgmp3-dev golang qtbase5-dev qtbase5-private-dev libqt5opengl5-dev qtdeclarative5-dev qml-module-qtquick-controls qml-module-qtquick-dialogs libqt5webengine5-dev
```

Finally, build the `mist` program using the following command.

```shell
$ cd go-ethereum
$ make mist
[ ... build output ... ]
Done building.
Run "build/bin/mist --asset_path=cmd/mist/assets" to launch mist.
```

Mist does not automatically look in the right location for its GUI
assets. You can set the asset path on the command line.

```shell
$ build/bin/mist --asset_path cmd/mist/assets
```

Or launch it from its source directory instead:

```shell
$ cd cmd/mist
$ ../../build/bin/mist
```