_**Note:** There are some upstream bugs that may prevent Mist from running correctly within VirtualBox in certain scenarios. See https://www.virtualbox.org/ticket/12746 and https://bugreports.qt.io/browse/QTBUG-43110_

## Building for Windows

[Building Instructions for Windows](https://github.com/ethereum/go-build#windows)

## Building for OSX

[Building Instructions for Mac OS](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)

## Building for Linux

[Building Instructions for Ubuntu](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Ubuntu)

## Running in Docker

The develop branch is automatically updated in Docker Hub with RPC exposed. To use it, ensure docker is installed correctly and execute the following commands:
```
docker pull ethereum/client-go
docker run -p 8545:8545 -p 30303:30303 ethereum/client-go
```

## Building the latest

Ethereum is under rapid development, and though it may sound contradictory, using the latest can be more stable and gratifying than using an older build.
To join the party, follow the [Developers' Guide](https://github.com/ethereum/go-ethereum/wiki/Developers%27-Guide)