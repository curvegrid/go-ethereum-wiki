## Option 1: Install from PPA

For the latest development snapshot, both `ppa:ethereum/ethereum` and `ppa:ethereum/ethereum-dev` are needed. If you want the stable version from the last PoC release, simply omit the `-dev` one.

**Warning: The `ethereum-qt` PPA will upgrade your system-wide Qt5 installation, from 5.2 on Trusty and 5.3 on Utopic, to 5.4.**

```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:ethereum/ethereum-qt
sudo add-apt-repository ppa:ethereum/ethereum
sudo add-apt-repository ppa:ethereum/ethereum-dev
sudo apt-get update
sudo apt-get install ethereum
```

Run `mist` for the GUI or `ethereum` for the CLI.

You can alternatively install only the CLI or GUI, with `apt-get install ethereum-cli` or `apt-get install mist` respectively.

## Option 2: Automatic installation

**Note** Outdated, please use the PPA.

[This Mist install script](https://gist.github.com/tgerring/d4ab3f1672ed91a53c6c) will install everything required from a fresh Ubuntu 14.04 installation and start running Mist.

```
wget https://gist.githubusercontent.com/tgerring/d4ab3f1672ed91a53c6c/raw/677a3dd9c6db099eee620657bf7fb1e664173ee1/mist-develop.sh -O install
chmod +x install 
./install
```

## Option 3: Manual build from source

### Installing Go

Verify that Go is installed by running `go version` and checking the version. If not, see [Installing Go](https://github.com/ethereum/go-ethereum/wiki/Installing-Go)

### Prerequisites

Mist depends on the following external libraries to be installed:
* [GMP](https://gmplib.org)
* [Readline](http://www.gnu.org/s/readline/)
* [Qt 5.4](http://www.qt.io/download-open-source/).

First install GMP & Readline from repositories:
```
sudo apt-get install -y libgmp3-dev libreadline6-dev
```
Second, follow the instructions for [Building Qt](https://github.com/ethereum/go-ethereum/wiki/Building-Qt)

### Installing Mist
At last you will now be finished with all the prerequisites. The following commands will build the Ethereum Mist GUI client for you:

    go get -u github.com/ethereum/go-ethereum/cmd/mist

(You may need to run "sudo apt-get install mercurial" first)

**Note**: Mist does not automatically look in the right location for its GUI assets. For this reason you have to launch it from its build directory

    cd $GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist && mist

or supply an absolute `-asset_path` option:

    mist -asset_path $GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist/assets


To eliminate the need to remember this cumbersome command, you can create the following a file in $GOPATH/bin :

    #!/bin/bash
    cd $GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist && mist

Name the file 'misted' and make it executable:

    chmod +x $GOPATH/bin/misted

Now mist can be run with the command `misted`