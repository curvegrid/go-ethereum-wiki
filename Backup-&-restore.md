Default storage locations:

* Mac: `~/Library/Ethereum`
* Linux: `~/./ethereum`
* Windows: `~/AppData/Roaming/Ethereum"`

Accounts are stored in the `keys` subdirectory. The contents of this directory should be transportable between platforms, but is specific to the Go implementation.

To configure the location of Geth's files, the `--datadir` parameter can be specified. See [CLI Options](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options) for more details.