### Windows 

Download and run the installer found at http://golang.org/doc/install


### OS X
Please refer to the [build instruction for OSX](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)

### Ubuntu

Download the latest distribution

`curl -O https://go.googlecode.com/files/go1.2.linux-amd64.tar.gz`

Unpack it to the `/usr/local` (might require sudo)

`tar -C /usr/local -xzf go1.2.linux-amd64.tar.gz`

`export PATH=$PATH:/usr/local/go/bin`
`mkdir ~/go; export GOPATH=$HOME/go`

#### Ubuntu 14.04 

The apt-get repositories for 14.04 contain golang 1.2.1. You can use `sudo apt-get install golang` from 14.04 rather than having to download and build from source as above. You will still have to set the $GOPATH and $PATH variables as above.