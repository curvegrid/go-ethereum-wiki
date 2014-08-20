QML API

The QML API is similar to the [PoC 6 Javascript API](). All methods can be shared across both implementations. Please note that this page will list methods which haven't been adopted by the Javascript API yet and should be considered *proposed*.

The QML API has one magic object `eth`. The eth object exposes method and variables from the *ethereum* browser to the QML interface.

## QML Javascript API

* `getBlock ( number_or_hash )`: Retrieves a block in the canonical chain by the given `number` or `hash`. `-1` may be given to get the latest block.
* `eachStorageAt ( address, callback )`: Iterates over the storage of the given object's `address` yielding `callback` for each key / pair.
* `key ( )`: Returns the current key object. A `Key` object consist out of the following fields:
  * `address`: address associated with this key.
  * `privateKey`: the private key that belongs to this key pair.

The API also adds a helper function not directly related to the state of ethereum or it's block chain.

* `fromNumber ( hex )`: Casts the given value passed by `hex` to a Big integer and returns it as string.

Filtering of `messages` is done in a slightly different way and requires the QML file to import the `filter.js` file found in `go-ethereum/assets/ext`. The filtering API allows you to create new filtering objects through which you watch for changes with the given filtering options in the following manner:

```qml
import "go/src/github.com/ethereum/go-ethereum/ethereal/assets/ext/filter.js" as Eth

var filter = new Eth.Filter({latest: -1, to: "cc6b22b87c8d1b607c92f4870f70e20658112f96"});
filter.changed(function(messages) {
    console.log(messages);
});
```

## QML API

Qml files loaded as a plugin within `ethereal` will be added as a tab to the sidebar. Qml files can customise the sidebar:

![](https://photos-3.dropbox.com/t/0/AAC-jzgMWKUGG7dvXSfPCCDXUsF1KTP0In2RnzjD8eJF_w/12/4270001/png/1024x768/3/1408550400/0/2/reqs.png/Uf_HhuRTcKGNhvuS6dbgJpdhiKqbbMVtkSpAZwqQnYA)

1. `iconSource`
2. `title`
3. `menuItem.secondaryTitle`

**Note:** When a QML file it's loaded it expects an `iconSource` and `title` and therefor must be specified. If you don't include them your QML file will fail to load.

When your QML file is loaded it will call the `onLoad` function if it's present and `onDestroy` when your application is closed. 

### Example

```qml
import QtQuick 2.0

Rectangle {
    id: root
    property var title: "Your title"
    property var iconSoruce: "path/to/icon.png"
    property var menuItem
    
    function onReady() {
        // Set the secondary title (3.)
        menuItem.secondaryTitle = "Hello world";        
    }
    
    Text {
        x: parent.width / 2
        y: parent.height / 2
        
        text: "Hello world"
    }
}
```