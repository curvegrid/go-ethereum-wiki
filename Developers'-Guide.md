**NOTE: These instructions are for people who want to contribute Go source code changes.
If you just want to run ethereum, use the normal [Installation Instructions](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum)**

# Developers' guide 

This document is the entry point for developers of the etherum go implementation. Developers here refer to the hands-on: who are interested in build, develop, debug, submit a bug report or pull request or contribute to `go-ethereum` with code. 

## Build and Test

### Go environment  

We assume that you have [`go` v1.7 installed](https://github.com/ethereum/go-ethereum/wiki/Installing-Go), and `GOPATH` is set.

**Note**:You must have your working copy under `$GOPATH/src/github.com/ethereum/go-ethereum`.

Since `go` does not use relative path for import, working in any other directory will have no effect, since the import paths will be appended to `$GOPATH/src`, and if the lib does not exist, the version at master HEAD will be downloaded.

Most likely you will be working from your fork of `go-ethereum`, let's say from `github.com/nirname/go-ethereum`. Clone or move your fork into the right place:

```
git clone git@github.com:nirname/go-ethereum.git $GOPATH/src/github.com/ethereum/go-ethereum
```

### Updating non-vendored Dependencies

Most go-ethereum dependencies are tracked in the vendor/ directory, with one exception:

The golang.org/x/net/context package needs to be installed using `go get`. To do that, run

```
go get
```

in the repository root after cloning the repository.

### Building executables

Switch to the go-ethereum repository root directory.

You can build all code using the go tool, placing the resulting binary in `$GOPATH/bin`.

```text
go install -v ./...
```

go-ethereum exectuables can be built individually. To build just geth, use:

```text
go install -v ./cmd/geth
```

Read about cross compilation of go-ethereum [here](https://github.com/ethereum/go-ethereum/wiki/Cross-compiling-Ethereum).

### Git flow

To make life easier try [git flow](http://nvie.com/posts/a-successful-git-branching-model/) it sets this all up and streamlines your work flow.

### Testing

Testing one library:

```
go test -v -cpu 4 ./eth  
```

Using options `-cpu` (number of cores allowed) and `-v` (logging even if no error) is recommended.

Testing only some methods:

```
go test -v -cpu 4 ./eth -run TestMethod
```

**Note**: here all tests with prefix _TestMethod_ will be run, so if you got TestMethod, TestMethod1, then both!

Running benchmarks, eg.:

```
go test -v -cpu 4 -bench . -run BenchmarkJoin
```

for more see [go test flags](http://golang.org/cmd/go/#hdr-Description_of_testing_flags)

See integration testing information on the [Testing wiki page](https://github.com/ethereum/go-ethereum/wiki/Testing)

### Metrics and monitoring

`geth` can do node behaviour monitoring, aggregation and show performance metric charts. 
Read about [metrics and monitoring](https://github.com/ethereum/go-ethereum/wiki/Metrics-and-Monitoring)

## Contributing

Only github is used to track issues. (Please include the commit and branch when reporting an issue.)

Pull requests should by default commit on the `master` branch.

## Stacktrace

The code uses `pprof` on localhost port 6060 by default if `geth` is started with the `--pprof` option. So bring up http://localhost:6060/debug/pprof to see the heap, running routines etc. By clicking full goroutine stack dump (clicking http://localhost:6060/debug/pprof/goroutine?debug=2) you can generate trace that is useful for debugging.

Note that if you run multiple instances of `geth`, this port will only work for the first instance that was launched. If you want to generate stacktraces for these other instances, you need to start them up choosing an alternative pprof port. Make sure you are redirecting stderr to a logfile. 

```
geth -port=30300 -loglevel 5 --pprof --pprofport 6060 2>> /tmp/00.glog
geth -port=30301 -loglevel 5 --pprof --pprofport 6061 2>> /tmp/01.glog
geth -port=30302 -loglevel 5 --pprof --pprofport 6062 2>> /tmp/02.glog
```

Alternatively if you want to kill the clients (in case they hang or stalled synching, etc) but have the stacktrace too, you can use the `-QUIT` signal with `kill`:

```
killall -QUIT geth 
```

This will dump stracktraces for each instance to their respective log file.

## Code formatting 

Sources are formatted according to the [Go Formatting
Style](http://golang.org/doc/effective_go.html#formatting).

## Dev Tutorials 

* [Private networks, local clusters and monitoring](https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster)

* [P2P 101](https://github.com/ethereum/go-ethereum/wiki/Peer-to-Peer): a tutorial about setting up and creating a p2p server and p2p sub protocol.

* [How to Whisper](https://github.com/ethereum/go-ethereum/wiki/How-to-Whisper): an introduction to whisper.