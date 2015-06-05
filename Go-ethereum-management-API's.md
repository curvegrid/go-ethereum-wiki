**Please note, this is work in progress and not yet available!**

# Overview
Beside the official [DApp API](https://github.com/ethereum/wiki/wiki/JSON-RPC) interface the go ethereum node has support for additional management API's. These API's are offered using [JSON-RPC](http://www.jsonrpc.org/specification) and follow the same conventions as used in the DApp API. The go ethereum package comes with a console client which has support for all additional API's.

# How to
It is possible to specify the set of API's which are offered with the `--${interface}api` command line argument for the go ethereum daemon. Where `${interface}` can be `rpc` for the official DApp API or `ipc`.

For example, `geth --ipcapi "admin,eth,miner" --rpcapi "eth,web3"` will
* enable the admin, official DApp and miner API over the IPC interface
* enable the eth and web3 API over the official RPC interface

Please note that offering a API over the `rpc` interface will give everyone access to the admin interface who can access the `rpc` interface (e.g. DApp's). So be careful which API's are offered over an interface.

By default geth enables all API's over the `ipc` interface and only the eth and web3 API's over the official `rpc` interface.

## Integration
These additional API's follow the same conventions the official DApp API. Web3 can be [extended](https://github.com/ethereum/web3.js/pull/229) and used to consume these additional API's. 

# API's
The management functions are split into multiple smaller logically grouped API's.

## Admin

## Debug

## Eth
This is the official DApp API. See for more information [this page](https://github.com/ethereum/wiki/wiki/JSON-RPC).

## Miner
Allows full control over the miner and [DAG](https://github.com/ethereum/wiki/wiki/Ethash-DAG).
* [start](#miner_start)
* [stop](#miner_stop)
* [hashrate](#miner_hashrate)
* [setExtra](#miner_setExtra)
* [setGasPrice](#miner_setGasPrice)
* [startAutoDAG](#miner_startAutoDAG)
* [stopAutoDAG](#miner_stopAutoDAG)
* [makeDAG](#miner_makeDAG)

### Net

### Web3


### miner_start
This will generates the DAG if necessary and starts the miner

#### Parameters
* `THREADS`, an option integer which specifies the number of threads, if not specified the number of CPU's is used

#### Return
`boolean` indicating if the miner was started

### miner_stop
This will stop the miner

#### Parameters
none

#### Return
`boolean` indicating if the miner was stopped

### miner_hashrate
Miner hashrate

#### Parameters
none

#### Return
`integer` in hashes p/s

### miner_setExtra
Store additional data in a mined block

#### Parameters
`DATA` string with extra data (max 1024 bytes)

#### Return
`boolean` indication if the DATA was set

### miner_setGasPrice
Set the gas price.

#### Parameters
`Price` string with new price, this can be a base8 (start with 0b), base10 (no prefix) or base16 representation (start with 0x)

#### Return
`boolean` indication if the new price was set

### miner_startAutoDAG
Pregenerate the DAG, this will allow for a seamless transition between the different epochs. If not enables the miner will need to generate the DAG when a new epoch begins. This can take some time will the miner will need to wait for it to finish.

#### Parameters
none

#### Return
`boolean` indication if the command was successful

### miner_stopAutoDAG
Stop DAG pregeneration.

#### Parameters
none

#### Return
`boolean` indication if the command was successful

### miner_makeDAG
Start the DAG creator process.

#### Parameters
none

#### Return
`boolean` indication if the command was successful
