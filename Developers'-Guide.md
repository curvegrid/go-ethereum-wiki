# Build and test

## Go environment  

We assume that you have [`go` installed](https://github.com/ethereum/go-ethereum/wiki/Installing-Go), `GOPATH` is set.

**Note**:You must have your working copy under `$GOPATH/src/github.com/ethereum/go-ethereum`. You also usually want to checkout the `develop` branch (instead of master).

Since `go` does not use relative path for import, working in any other directory will have no effect, since the import paths will be appended to `$GOPATH/src`, and if the lib does not exist, the version at master HEAD will be downloaded.

Most likely you will be working from your fork of `go-ethereum`, let's say from `github.com/nirname/go-ethereum`. Clone or move your fork into the right place:

```
git clone git@github.com:nirname/go-ethereum.git $GOPATH/src/github.com/ethereum/go-ethereum
```

## Godep for dependency management
go-ethereum uses [Godep](https://github.com/tools/godep) to manage dependencies.

Install godep: 

```
go get github.com/tools/godep
```

Make sure that go binaries are on your executable path:

```
PATH=$GOPATH/bin:$PATH
```

`godep` should be prepended to all go calls `build`, `install` and `test`. 

Alternatively, you can prepend the go-ethereum Godeps directory to your current `GOPATH`:

```
GOPATH=`godep path`:$GOPATH
```

## Building executables

Switch to the go-ethereum repository root directory (Godep expects a local [Godeps folder](https://github.com/ethereum/go-ethereum/tree/develop/Godeps) ).

Each wrapper/executable found in 
[the `cmd` directory](https://github.com/ethereum/go-ethereum/tree/develop/cmd) can be built individually.

## Building Geth (CLI)

**Note**: Geth (the ethereum command line client) is the focus of the [Frontier release](https://github.com/ethereum/go-ethereum/wiki/Frontier).

To build the CLI:

```
godep go install -v ./cmd/geth
```

See the [documentation on how to use Geth](https://github.com/ethereum/go-ethereum/wiki/Geth)

## Building Mist (GUI)

**Note**: Mist is not officially released as part of [Frontier](https://github.com/ethereum/go-ethereum/wiki/Frontier)

For the GUI, you need to [install `QT5`](https://github.com/ethereum/go-ethereum/wiki/Building-Qt) and set variables.

On OSX with a brew install of `qt5`:

``` bash
export PKG_CONFIG_PATH=/usr/local/Cellar/qt5/5.4.0/lib/pkgconfig
export CGO_CPPFLAGS=-I/usr/local/Cellar/qt5/5.4.0/include/QtCore/5.4.0/QtCore
export LD_LIBRARY_PATH=/usr/local/Cellar/qt5/5.4.0/lib
```

See the instructions on the [wiki](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum%28Go%29)

With these prerequisites in place, compile `mist` with:

```
godep go build -v ./cmd/mist
```

Mist does not automatically look in the right location for its GUI assets. For this reason you need to launch it from its build directory 

    cd $GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist && mist

or supply an absolute `-asset_path` option:

    mist -asset_path $GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist/assets

## Git flow

To make life easier try [git flow](http://nvie.com/posts/a-successful-git-branching-model/) it sets this all up and streamlines your work flow.

## Testing

Testing one library:

```
godep go test -v -cpu 4 ./eth  
```

Using options `-cpu` (number of cores allowed) and `-v` (logging even if no error) is recommended.

Testing only some methods:

```
godep go test -v -cpu 4 ./eth -run TestMethod
```

**Note**: here all tests with prefix TestMethod will be run, so if you got TestMethod, TestMethod1, then both!

Running benchmarks, eg.:

```
cd bzz
godep go test -v -cpu 4 -bench . -run BenchmarkJoin
```

for more see [go test flags](http://golang.org/cmd/go/#hdr-Description_of_testing_flags)

## Add and update dependencies 

To update a dependency version (for example, to include a new upstream fix), run 

```
go get -u <foo/bar>
godep update <foo/...>
```

To track a new dependency, add it to the project as normal than run 

```
godep save ./...
```

Changes to the [Godeps folder](https://github.com/ethereum/go-ethereum/tree/develop/Godeps) should be manually verified then committed.

To make life easier try [git flow](http://nvie.com/posts/a-successful-git-branching-model/) it sets this all up and streamlines your work flow.

# Contributing

Only github is used to track issues. (Please include the commit and branch when reporting an issue.)

Pull requests should by default commit on the `develop` branch.
The `master` branch is only used for finished stable major releases.

# Stacktrace

The code uses `pprof` on localhost port 6060. So bring up `localhost:6060/debug/pprof` to see the heap, running routines etc. By clicking full goroutine stack dump (clicking http://localhost:6060/debug/pprof/goroutine?debug=2) you can generate trace that is useful for debugging.

Note that if you run multiple instances of `geth`, this port will only work for the first instance that was launched. If you want to generate stacktraces for these other instances, you need to send it a `-QUIT` signal. Make sure you are redirecting stderr to a logfile. 

```
geth -port=30300 -loglevel 5 2>> /tmp/00.glog
geth -port=30301 -loglevel 5 2>> /tmp/01.glog
geth -port=30302 -loglevel 5 2>> /tmp/02.glog
killall -QUIT geth 
```

This will dump stracktraces for each instance to their respective log file.

# Code formatting 

Sources are formatted according to the [Go Formatting
Style](http://golang.org/doc/effective_go.html#formatting).

# Dev Tutorials 

* [Private networks, local clusters and monitoring](https://github.com/ethereum/go-ethereum/wiki/Setting-up-private-network-or-local-cluster)

* [P2P 101](https://github.com/ethereum/go-ethereum/wiki/Peer-to-Peer): a tutorial about setting up and creating a p2p server and p2p sub protocol.

* [How to Whisper](https://github.com/ethereum/go-ethereum/wiki/How-to-Whisper): an introduction to whisper.

