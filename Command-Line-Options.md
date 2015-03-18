# CLI client

Command line client options are a moving target under constant change now. Please refer to the clients help option. Output of `ethereum help` (version 0.9.1, 2015.03.18). 

```
ethereum [global options] command [command options] [arguments...]

VERSION:
   0.9.1

COMMANDS:
   blocktest    loads a block test file
   version      print ethereum version numbers
   account      manage accounts
   dump         dump a specific block from storage
   console      Ethereum Console: interactive JavaScript environment
   js           executes the given JavaScript files in the Ethereum Frontier JavaScript VM
   import       import a blockchain file
   export       export blockchain into file
   help, h      Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --unlock                                     Unlock a given account untill this programs exits (address:password)
   --bootnodes                                  Space-separated enode URLs for discovery bootstrap
   --datadir "$HOME/Library/Ethereum"           Data directory to be used
   --jspath "."                                 JS library path to be used with console and js subcommands
   --port "30303"                               Network listening port
   --logfile                                    Send log output to a file
   --logformat "std"                            "std" or "raw"
   --loglevel "3"                               0-5 (silent, error, warn, info, debug, debug detail)
   --maxpeers "16"                              Maximum number of network peers
   --minerthreads "8"                           Number of miner threads
   --mine                                       Enable mining
   --nat "any"                                  Port mapping mechanism (any|none|upnp|pmp|extip:<IP>)
   --nodekey                                    P2P node key file
   --nodekeyhex                                 P2P node key as hex (for testing)
   --rpc                                        Whether RPC server is enabled
   --rpcaddr "127.0.0.1"                        Listening address for the JSON-RPC server
   --rpcport "8545"                             Port on which the JSON-RPC server should listen
   --unencrypted-keys                           disable private key disk encryption (for testing)
   --vmdebug                                    Virtual Machine debug output
   --protocolversion "58"                       ETH protocol version
   --networkid "0"                              Network Id
   --help, -h                                   show help

```

Note that the default for datadir is platform-specific, "/$HOME/Library/Ethereum" is the pattern for MacOS. 

## Examples: 

executes `test.js` javascript using js API and logs Debug level log messages to `/path/to/logfile`:

    ethereum -logfile /path/to/logfile -loglevel js test.js  # 
   
imports a blockchain from file (used to programmatically set address from script):

    ethereum import blockchain.bin

start two mining nodes with their own client ids using different data directories listening on ports 30300 and 30301 respectively:

    ethereum -mine -minerthreads 4 -datadir /usr/local/share/ethereum/30303 -port 30303
    ethereum -mine -minerthreads 4 -datadir /usr/local/share/ethereum/30304 -port 30304
    
start an rpc client on port 8000:

    ethereum -rpc true -rpcport 8000

Start a client and connect to specific peers:

    ethereum -bootnodes="enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303 enode://0b2fa1e93dfcd90ce503ab1338332d6a1371b464838b49f37af8534a510992bd4d96b24134ba262ad9298ab4aa6f132132f84c3b6d10ebaead5f9a236be286f10@54.169.166.218:30305" -maxpeers 2


## GUI Client 

The output of `mist help`. Please refer to the client for uptodate info. 

```
mist [global options] command [command options] [arguments...]

VERSION:
   0.9.0

COMMANDS:
   help, h      Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --asset_path "$GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist/assets"      absolute path to GUI assets directory
   --bootnodes                                                                                          Space-separated enode URLs for discovery bootstrap
   --datadir "/Users/tron/Library/Ethereum"                                                             Data directory to be used
   --port "30303"                                                                                       Network listening port
   --logfile                                                                                            Send log output to a file
   --loglevel "3"                                                                                       0-5 (silent, error, warn, info, debug, debug detail)
   --maxpeers "16"                                                                                      Maximum number of network peers
   --minerthreads "8"                                                                                   Number of miner threads
   --nat "any"                                                                                          Port mapping mechanism (any|none|upnp|pmp|extip:<IP>)
   --nodekey                                                                                            P2P node key file
   --rpcaddr "127.0.0.1"                                                                                Listening address for the JSON-RPC server
   --rpcport "8545"                                                                                     Port on which the JSON-RPC server should listen
   --jspath "."                                                                                         JS library path to be used with console and js subcommands
   --protocolversion "58"                                                                               ETH protocol version
   --networkid "0"                                                                                      Network Id
   --help, -h                                                                                           show help
   --version, -v                                                                                        print the version

```

Example: 

    mist --asset_path /absolute/path/to/assets

## Alternative ways to set flags

**WARNING:** This is not available for the latest frontier poc9.

The same flags can be set via config file (by default `<datadir>/conf.ini`) as well as environment variables. 

**Precedence**: default < config file < environment variables < command line


