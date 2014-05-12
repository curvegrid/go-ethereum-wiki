### Windows 

Download and run the installer found at http://golang.org/doc/install


### OS X

For simplicity down the line, it is recommended to install Go via brew rather than installing via http://golang.org/doc/install

Install brew:

`ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"`

Install Go:

`brew update`
`brew install go`

You need a go directory to place your Go apps, so create it. You can call it whatever your want, most people choose to call it 'go', funny enough

`mkdir ~/go`

Now set the $GOPATH to the directory you just created. $GOPATH is where Go will look for your Go apps

`export GOPATH=$HOME/go`

This will only work for the duration of your session, so save it to your bash profile so it sticks

`echo GOPATH=$GOPATH >> ~/.bash_profile`


### Ubuntu

Download the latest distribution

`curl -O https://go.googlecode.com/files/go1.2.linux-amd64.tar.gz`

Unpack it to the `/usr/local` (might require sudo)

`tar -C /usr/local -xzf go1.2.linux-amd64.tar.gz`

`export PATH=$PATH:/usr/local/go/bin`
`mkdir ~/go; export GOPATH=$HOME/go`