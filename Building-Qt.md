Ethereum(Go) Requires QML 5.1+

## Mac OS X

`brew tap homebrew/versions`

`brew install pkg-config qt5`

Qt >=5.1 is needed.
The following commands set the current Qt version and use it to set a go compiler flag:

    export PKG_CONFIG_PATH=`brew --prefix qt5`/lib/pkgconfig
    export QT5VERSION=`pkg-config --modversion Qt5Core`
    export CGO_CPPFLAGS=-I`brew --prefix qt5`/include/QtCore/$QT5VERSION/QtCore

then go-ethereum build should work 

    go get -u github.com/ethereum/go-ethereum
    go build -v 

## Linux

    sudo add-apt-repository ppa:ubuntu-sdk-team/ppa
    sudo apt-get update
    sudo apt-get install ubuntu-sdk qtbase5-private-dev qtdeclarative5-private-dev libqt5opengl5-dev