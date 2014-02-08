## Getting Go1.2 installed

### Ubuntu

`curl -O https://go.googlecode.com/files/go1.2.linux-amd64.tar.gz`

Unpack it to the `/usr/local` (might require sudo)

`tar -C /usr/local -xzf go1.2.linux-amd64.tar.gz`

### OS X

Update brew and install Go 1.2

`brew update; brew install go`

Create the Go dir and set the GOPATH

`mkdir ~/go; export GOPATH=~/go`