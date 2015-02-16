_**Note:** There are some upstream bugs that may prevent Mist from running correctly within VirtualBox in certain scenarios. See https://www.virtualbox.org/ticket/12746 and https://bugreports.qt.io/browse/QTBUG-43110_

## Building for Windows

[Building Instructions for Windows](https://github.com/ethereum/go-build#windows)

## Building for OSX

[Building Instructions for Mac OS](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)

## Building for Linux

[Ubuntu 14.04](https://github.com/ethereum/go-ethereum/wiki/Instructions-for-getting-the-Go-implementation-of-Ethereum-and-the-Mist-browser-installed-on-Ubuntu-14.04-(trusty))

## Running in Docker

The develop branch is automatically updated in Docker Hub with RPC exposed. To use it, ensure docker is installed correctly and execute the following commands:
```
docker pull ethereum/client-go
docker run -p 8045:8045 -p30303:30303 ethereum/client-go
```