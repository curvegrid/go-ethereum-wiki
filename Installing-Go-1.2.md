## Getting Go1.2 installed

### Windows 

Download and run the installer at http://golang.org/doc/install


### OS X

Update brew and install Go 1.2 

`brew update; brew install go`

(alternatively, you can also install Go via http://golang.org/doc/install)

Create the Go dir and set the GOPATH

`mkdir ~/go; export GOPATH=$HOME/go`


### Ubuntu

`curl -O https://go.googlecode.com/files/go1.2.linux-amd64.tar.gz`

Unpack it to the `/usr/local` (might require sudo)

`tar -C /usr/local -xzf go1.2.linux-amd64.tar.gz`

`export PATH=$PATH:/usr/local/go/bin`
`mkdir ~/go; export GOPATH=$HOME/go`