### Windows 

Download and run the installer found at http://golang.org/doc/install


### OS X

Install brew:
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"

Install Go:
`brew update; brew install go`

You need a go directory to place your Go apps, so create a Go dir (you can call it whatever your want) and set the GOPATH

`mkdir ~/go; export GOPATH=$HOME/go`

Note: it is recommend to use the technique above rather than installing via http://golang.org/doc/install


### Ubuntu

Download the latest distribution

`curl -O https://go.googlecode.com/files/go1.2.linux-amd64.tar.gz`

Unpack it to the `/usr/local` (might require sudo)

`tar -C /usr/local -xzf go1.2.linux-amd64.tar.gz`

`export PATH=$PATH:/usr/local/go/bin`
`mkdir ~/go; export GOPATH=$HOME/go`