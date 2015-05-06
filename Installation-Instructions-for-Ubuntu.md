## Installing from PPA

For the latest development snapshot, both `ppa:ethereum/ethereum` and `ppa:ethereum/ethereum-dev` are needed. If you want the stable version from the last PoC release, simply omit the `-dev` one.

*Warning: The `ethereum-qt` PPA will upgrade your system-wide Qt5 installation, from 5.2 on Trusty and 5.3 on Utopic, to 5.4.*

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

### Building Geth (command line client)

Clone the repository to a directory of your choosing:

```shell
git clone https://github.com/ethereum/go-ethereum
```

Building `geth` requires some external libraries to be installed:

```shell
sudo apt-get install -y build-essential libgmp3-dev golang
```

Finally, build the `geth` program using the following command.
```shell
cd go-ethereum
make geth
```

You can now run `build/bin/geth` to start your node.
