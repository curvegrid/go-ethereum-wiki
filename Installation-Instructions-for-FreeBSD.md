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
pkg install gmp go
```

If your golang version is >= 1.5, build the `geth` program using the following command.
```shell
cd go-ethereum
make geth
```
If your golang version is < 1.5 (quarterly packages, for example), use the following command instead.
```shell
cd go-ethereum
CC=clang make geth
```


You can now run `build/bin/geth` to start your node.
