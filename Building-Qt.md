Ethereum(Go) Requires QML 5.1+

## Mac OS X

`brew tap homebrew/versions`

`brew install qt5`

The following commands assume Qt 5.2.0. Please check your current Qt version

    export QT5VERSON=5.2.0
    export PKG_CONFIG_PATH=`brew --prefix qt5`/lib/pkgconfig

    # For "private/qmetaobject_p.h" inclusion
    export CGO_CPPFLAGS=-I`brew --prefix qt5`/include/QtCore/$QT5VERSION/QtCore
    go get github.com/niemeyer/qml