We assume you have a GOPATH set up appropriately. If not, you can read about setting it up here: http://golang.org/doc/code.html#GOPATH.

Note: Using a separate GOPATH for ethereum development may provide a cleaner development environment.

In order to 'build edge', we will want to use the `develop` branch of the go-ethereum repo. This will likely require using the `develop` branch of the eth-go backend, as well.

* Begin by getting the newest version of go-ethereum and its dependencies:

        go get -u github.com/ethereum/go-ethereum

* Switch to the `develop` branch on both the eth-go and go-ethereum src directories, and reinstall:

        cd $GOPATH/src/github.com/ethereum/eth-go
        git checkout develop
        go install
        cd $GOPATH/src/github.com/ethereum/go-ethereum
        git checkout develop
        go install

* go-ethereum should now be built in `$GOPATH/bin/`. Run it:

        $GOPATH/bin/go-ethereum