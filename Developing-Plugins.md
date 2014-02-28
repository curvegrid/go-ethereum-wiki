Ethereal supports third-party plugins in the form of [QML](http://qt-project.org/doc/qt-5/index.html) files. The current support Qt version is 5.2.

Ethereal exposes several interfaces and functions to QML.

The `eth` interface exposes Ethereum specific functions:

* createTx(receiver, address, data) [string]
* getBlock(hash) [block]

The `ui` interface exposes GUI specific functions:

* assetPath(asset) [string]

## Examples

* [NameReg](https://github.com/ethereum/go-ethereum/wiki/Example_NameReg)