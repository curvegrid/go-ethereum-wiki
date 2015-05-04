## Installation Instructions

Follow the appropriate link below to find installation instructions for
your platform.

* [Installation Instructions for Mac OS X](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Mac)
* Linux
  * [Installation Instructions for Ubuntu](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu)
* [Installation Instructions for Windows](https://github.com/ethereum/go-ethereum/wiki/Installation-instructions-for-Windows)

_**Note:** There are some upstream bugs that may prevent Mist from running correctly within VirtualBox in certain scenarios. See https://www.virtualbox.org/ticket/12746 and https://bugreports.qt.io/browse/QTBUG-43110_

## Running in Docker

We keep a Docker image with recent snapshot builds from the `develop` branch [on DockerHub](https://registry.hub.docker.com/u/ethereum/client-go). Run this first:

```shell
docker pull ethereum/client-go
```

To start a node that runs the JSON-RPC interface on port **8545**, run:

```shell
docker run -p 8545:8545 -p 30303:30303 ethereum/client-go
```

To use the interactive JavaScript console, run:

```shell
docker run -it --entrypoint="/usr/bin/geth" ethereum/client-go console
```