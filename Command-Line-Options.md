# CLI client

```
    ethereum [options] [filename]
```

Options:

```
  -bootnodes="": space-separated node URLs for discovery bootstrap
  -chain="": Imports given chain
  -conf="<datadir>/conf.ini": config file
  -datadir="$HOME/Library/Ethereum": specifies the datadir to use
  -debug="": debug file (no debugging if not set)
  -dial=true: dial out connections (default on)
  -diff="all": sets the level of diff output [vm, all]. Has no effect if difftool=false
  -difftool=false: creates output for diff'ing. Sets LogLevel=0
  -dump=false: output the ethereum state in JSON format. Sub args [number, hash]
  -export="": exports the session keyring to files in the directory given
  -genaddr=false: create a new priv/pub key
  -genesis=false: Dump the genesis block
  -hash="": specify arg in hex
  -id="": Custom client identifier
  -import="": imports the file given (hex or mnemonic formats)
  -js=false: launches javascript console
  -keyring="": identifier for keyring to use
  -keystore="db": system to store keyrings: db|file 
  -logfile="": log file (defaults to standard output)
  -logformat="std": logformat: std,raw
  -loglevel=3: loglevel: 0-5 (= silent,error,warn,info,debug,debug detail)
  -maxpeer=30: maximum desired peers
  -mine=false: start dagger mining
  -minerthreads=8: number of miner threads
  -nat="any": port mapping mechanism (any|none|upnp|pmp|extip:<IP>)
  -nodekey="": network private key file
  -nodekeyhex="": network private key (for testing)
  -number=-1: specify arg in number
  -port="30303": listening port
  -rpc=false: start rpc server
  -rpcaddr="127.0.0.1": address for json-rpc server to listen on
  -rpcport=8545: port to start json-rpc server on
  -version=false: prints version number
  -vm=0: Virtual Machine type: 0-1: standard, debug
  -ws=false: start websocket server
  -wsport=40404: port to start websocket rpc server on
  -y=false: non-interactive mode (say yes to confirmations)

  filename   Load the given file and interpret as JavaScriptdefault value after =. Note the `=` is optional.

```

Note that the default for datadir is platform-specific, the example shown is for Mac OSX.


## Examples: 

executes `test.js` javascript using js API and logs to `/path/to/logfile`:

    ethereum -logfile /path/to/logfile test.js  # 
   
non-interactively imports a key and exits (used to programmatically set address from script):

    ethereum -y -import `cat mysecret.hex`

start two mining nodes with their own client ids using different data directories listening on ports 30300 and 30301 respectively:

    ethereum -nat=upnp -id ethersphere.0 -datadir /usr/local/share/ethereum/0 -port 30300
    ethereum -nat=upnp -id ethersphere.1 -datadir /usr/local/share/ethereum/1 -port 30301
    
start an rpc client on port 8000:

    ethereum -rpc true -rpcport 8000

## GUI Client 

    mist [options]
    
Options:

```
  -asset_path="/Users/tron/Work/ethereum/go/src/github.com/ethereum/Resources": absolute path to GUI assets directory
  -bootnodes="": space-separated node URLs for discovery bootstrap
  -conf="<datadir>/conf.ini": config file
  -datadir="$HOME/Library/Ethereum": specifies the datadir to use
 -debug="": debug file (no debugging if not set)
  -export="": exports the session keyring to files in the directory given
  -genaddr=false: create a new priv/pub key
  -id="": Custom client identifier
  -import="": imports the file given (hex or mnemonic formats)
  -keyring="": identifier for keyring to use
  -keystore="db": system to store keyrings: db|file
  -logfile="": log file (defaults to standard output)
  -loglevel=3: loglevel: 0-5 (= silent,error,warn,info,debug,debug detail)
  -maxpeer=30: maximum desired peers
  -minerthreads=8: number of miner threads
  -nat="any": port mapping mechanism (any|none|upnp|pmp|extip:<IP>)
  -nodekey="": network private key file
  -nodekeyhex="": network private key (for testing)
  -port="30303": listening port
  -rpc=true: start rpc server
  -rpcaddr="127.0.0.1": address for json-rpc server to listen on
  -rpcport=8545: port to start json-rpc server on
  -vm=0: Virtual Machine type: 0-1: standard, debug
  -ws=false: start websocket server
  -wsport=40404: port to start websocket rpc server on
  -y=false: non-interactive mode (say yes to confirmations)
```

default value after =. Note the `=` is optional.

Example: 

    mist --asset_path /absolute/path/to/assets

## Alternative ways to set flags

The same flags can be set via config file (by default `<datadir>/conf.ini`) as well as environment variables. 

**Precedence**: default < config file < environment variables < command line


