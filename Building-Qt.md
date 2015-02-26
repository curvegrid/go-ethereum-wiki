Ethereum(Go) Requires QML 5.4+

## Mac OS X

Please see [build instruction for OSX](https://github.com/ethereum/go-ethereum/wiki/Building-Instructions-for-Mac)

## Linux

### Ubuntu 14.04

```
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install pkg-config
sudo add-apt-repository ppa:beineri/opt-qt541-trusty
sudo apt-get update
sudo apt-get install -y qt54quickcontrols qt54webengine
source /opt/qt54/bin/qt54-env.sh
```

Now it's time to build Qt:

```
go get -u github.com/obscuren/qml
cd $GOPATH/src/github.com/obscuren/qml && git checkout v1
go build
```

If you receive an error about not being able to find Qt* items, check that PKG_CONFIG_PATH and LD_LIBRARY_PATH have been set (`echo $PKG_CONFIG_PATH` and `echo $LD_LIBRARY_PATH`) and if not, running `source /opt/qt54/bin/qt54-env.sh` will set the variables for the current session.