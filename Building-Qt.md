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

To install ubuntu-sdk, you may need to follow http://askubuntu.com/questions/408463/unmet-dependencies-i-cant-install-ubuntu-sdk

On Ubuntu 14.04, you will have to separately install the qtdialogs package

    sudo apt-get instsall qtdeclarative5-dialogs-plugin

If you get an error which looks like:

    FATAL: asset not found: you can set an alternative asset path on on the command line using option 'asset_path'
    panic: file:///$GOPATH/src/github.com/ethereum/go-ethereum/ethereal/assets/qml/wallet.qml:5 module "QtQuick.Window" version 2.1 is not installed

Then you will need to have >= QT5.1 installed and your environmental variables set to something like this:

    export PKG_CONFIG_PATH=<QT Location>/Qt5.2.1/5.2.1/gcc_64/lib/pkgconfig
    export CGO_CPPFLAGS="-I<QT Location>/Qt5.2.1/5.2.1/gcc_64/include/QtCore/5.2.1/QtCore"
    export LD_LIBRARY_PATH=<QT Location>/Qt5.2.1/5.2.1/gcc_64/lib

Remember to change <QT Location> to wherever you have QT installed. After setting your environmental variables, you will need to reinstall the go-qml package and the ethereal package:

    go get -u github.com/go-qml/qml
    go get -u github.com/ethereum/go-ethereum/ethereal

For 13.10, the qt packages in the ppas are not >= 5.1 at this time, so you must download and install QT from [here](http://qt-project.org/downloads). After you run the installer, then you can set the environment variables as described above and it should work as expected. 