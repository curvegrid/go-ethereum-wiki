[Installation Instructions for Windows](https://github.com/ethereum/go-build#windows)

[Installation Instructions for Mac OS](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)

[Installation Instructions for Ubuntu](https://github.com/ethereum/go-ethereum/wiki/Installation-Instructions-for-Ubuntu)

_**Note:** There are some upstream bugs that may prevent Mist from running correctly within VirtualBox in certain scenarios. See https://www.virtualbox.org/ticket/12746 and https://bugreports.qt.io/browse/QTBUG-43110_

## Running in Docker

We keep a Docker image with recent snapshot builds from the `develop` branch [on DockerHub](https://registry.hub.docker.com/u/ethereum/client-go). Run this first:

```
docker pull ethereum/client-go
```

To start a node that runs the JSON-RPC interface on port **8545**, run:

```
docker run -p 8545:8545 -p 30303:30303 ethereum/client-go
```

To use the interactive JavaScript console, run:

```
docker run -it --entrypoint="/usr/bin/geth" ethereum/client-go console
```

## Developing go-ethereum

If you want to implement features or fix bugs in go-ethereum, follow the [Developers' Guide](https://github.com/ethereum/go-ethereum/wiki/Developers%27-Guide)