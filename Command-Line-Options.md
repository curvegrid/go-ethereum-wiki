# CLI client

Command line client options are a moving target under constant change now. Please refer to the client to geth help. Below is the output of `geth help` (version 0.9.19 10/5/2015). 

```
geth [global options] command [command options] [arguments...]

VERSION:
   0.9.19

COMMANDS:
   blocktest    loads a block test file
   makedag      generate ethash dag (for testing)
   version      print ethereum version numbers
   wallet       ethereum presale wallet
   account      manage accounts
   dump         dump a specific block from storage
   console      Geth Console: interactive JavaScript environment
   js           executes the given JavaScript files in the Geth JavaScript VM
   import       import a blockchain file
   export       export blockchain into file
   upgradedb    upgrade chainblock database
   help         Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --identity                                   Custom node name
   --unlock                                     Unlock the account given until this program exits (prompts for password). '--unlock primary' unlocks the primary account
   --password                                   Path to password file to use with options and subcommands needing a password
   --bootnodes                                  Space-separated enode URLs for p2p discovery bootstrap
   --datadir "/Users/tron/Library/Ethereum"     Data directory to be used
   --blockchainversion "2"                      Blockchain version (integer)
   --jspath "."                                 JS library path to be used with console and js subcommands
   --port "30303"                               Network listening port
   --maxpeers "25"                              Maximum number of network peers (network disabled if set to 0)
   --maxpendpeers "0"                           Maximum number of pending connection attempts (defaults used if set to 0)
   --etherbase "primary"                        Public address for block mining rewards. By default the address of your primary account is used
   --gasprice "10000000000000"                  Sets the minimal gasprice when mining transactions
   --minerthreads "8"                           Number of miner threads
   --mine                                       Enable mining
   --nat "any"                                  NAT port mapping mechanism (any|none|upnp|pmp|extip:<IP>)
   --natspec                                    Enable NatSpec confirmation notice
   --nodekey                                    P2P node key file
   --nodekeyhex                                 P2P node key as hex (for testing)
   --rpc                                        Enable the JSON-RPC server
   --rpcaddr "127.0.0.1"                        Listening address for the JSON-RPC server
   --rpcport "8545"                             Port on which the JSON-RPC server should listen
   --shh                                        Enable whisper
   --vmdebug                                    Virtual Machine debug output
   --protocolversion "60"                       ETH protocol version (integer)
   --networkid "0"                              Network Id (integer)
   --rpccorsdomain                              Domain on which to send Access-Control-Allow-Origin header
   --verbosity "3"                              Logging verbosity: 0-6 (0=silent, 1=error, 2=warn, 3=info, 4=core, 5=debug, 6=debug detail)
   --backtrace_at ":0"                          If set to a file and line number (e.g., "block.go:271") holding a logging statement, a stack trace will be logged
   --logtostderr                                Logs are written to standard error instead of to files.
   --vmodule ""                                 The syntax of the argument is a comma-separated list of pattern=N, where pattern is a literal file name (minus the ".go" suffix) or "glob" pattern and N is a log verbosity level.
   --logfile                                    Send log output to a file
   --logjson                                    Send json structured log output to a file or '-' for standard output (default: no json output)
   --pprof                                      Enable the profiling server on localhost
   --pprofport "6060"                           Port on which the profiler should listen
   --solc "solc"                                solidity compiler to be used
   --help, -h                                   show help


```

Note that the default for datadir is platform-specific. See [backup & restore](https://github.com/ethereum/go-ethereum/wiki/Backup-&-restore) for more information.

## Examples

### Accounts
See [Account management](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts)

Import ether presale wallet into your node (prompts for password):

    geth wallet import /path/to/my/etherwallet.json

Import an EC privatekey into an ethereum account (prompts for password):

    geth account import /path/to/key.prv

### Geth JavaScript Runtime Environment 

See [Geth javascript console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)

Bring up the geth javascript console:

    geth --verbosity 5 --logtostderr --jspath /mydapp/js console 2>> /path/to/logfile

Execute `test.js` javascript using js API and log Debug-level messages to `/path/to/logfile`:

    geth --logtostderr --verbosity 6 js test.js  2>> /path/to/logfile

### Import/export chains and dump blocks

Import a blockchain from file:

    geth import blockchain.bin

### Upgrade chainblock database

When the consensus algorithm is changed blocks in the blockchain must be reimported with the new algorithm. Geth will inform the user with instructions when and how to do this when it's necessary.

    geth upgradedb

### Mining and networking

Start two mining nodes using different data directories listening on ports 30303 and 30304, respectively:

    geth --mine --minerthreads 4 --datadir /usr/local/share/ethereum/30303 --port 30303
    geth --mine --minerthreads 4 --datadir /usr/local/share/ethereum/30304 --port 30304
    
Start an rpc client on port 8000:

    geth --rpc true --rpcport 8000 --rpccorsdomain '"*"'

Start a client with a private network id and connect to specific peers using [enode urls](https://github.com/ethereum/wiki/wiki/enode-url-format):

    geth --networkid 3301 -bootnodes="enode://6f8a80d14311c39f35f516fa664deaaaa13e85b2f7493f37f6144d86991ec012937307647bd3b9a82abe2974e1407241d54947bbb39763a4cac9f77166ad92a0@54.169.166.226:30303 enode://0b2fa1e93dfcd90ce503ab1338332d6a1371b464838b49f37af8534a510992bd4d96b24134ba262ad9298ab4aa6f132132f84c3b6d10ebaead5f9a236be286f10@54.169.166.218:30305" -maxpeers 2

Launch the client without network:

    geth --maxpeers 0 js justwannarunthis.js

#### Resetting the blockchain

In the datadir, delete the blockchain directory.  For an example above:

    rm -rf /usr/local/share/ethereum/30303/blockchain

### Sample usage in testing environment

The lines below are meant only for test network and safe environments for non-interactive scripted use.

```
geth --datadir /tmp/eth/42 --password <(echo -n notsosecret) account new 2>> /tmp/eth/42.log
geth --datadir /tmp/eth/42 --port 30342  js <(echo 'console.log(admin.nodeInfo().NodeUrl)') > enode 2>> /tmp/eth/42.log
geth --datadir /tmp/eth/42 --port 30342 --password <(echo -n notsosecret) --unlock primary --minerthreads 4 --mine 2>> /tmp/eth/42.log
```

## GUI Client 

The output of `mist help`. Please refer to the client for uptodate info. 

```
mist [global options] command [command options] [arguments...]

VERSION:
   0.9.19

COMMANDS:
   help Shows a list of commands or help for one command

GLOBAL OPTIONS:
   --asset_path "$GOPATH/src/github.com/ethereum/go-ethereum/cmd/mist/assets"      absolute path to GUI assets directory
   --rpccorsdomain "http://localhost"                                                                   Domain on which to send Access-Control-Allow-Origin header
   --bootnodes                                                                                          Space-separated enode URLs for p2p discovery bootstrap
   --datadir "$HOME/Library/Ethereum"                                                             Data directory to be used
   --port "30303"                                                                                       Network listening port
   --logfile                                                                                            Send log output to a file
   --verbosity "3"                                                                                      Logging verbosity: 0-6 (0=silent, 1=error, 2=warn, 3=info, 4=core, 5=debug, 6=debug detail)
   --maxpeers "25"                                                                                      Maximum number of network peers (network disabled if set to 0)
   --maxpendpeers "0"                                                                                   Maximum number of pending connection attempts (defaults used if set to 0)
   --minerthreads "8"                                                                                   Number of miner threads
   --nat "any"                                                                                          NAT port mapping mechanism (any|none|upnp|pmp|extip:<IP>)
   --nodekey                                                                                            P2P node key file
   --rpcaddr "127.0.0.1"                                                                                Listening address for the JSON-RPC server
   --rpcport "8545"                                                                                     Port on which the JSON-RPC server should listen
   --jspath "."                                                                                         JS library path to be used with console and js subcommands
   --protocolversion "60"                                                                               ETH protocol version (integer)
   --blockchainversion "2"                                                                              Blockchain version (integer)
   --networkid "0"                                                                                      Network Id (integer)
   --help, -h                                                                                           show help
   --version, -v                                                                                        print the version

```

Example: 

    mist --asset_path /absolute/path/to/assets

## Alternative ways to set flags

**WARNING:** This is not available for the latest frontier poc9.

The same flags can be set via config file (by default `<datadir>/conf.ini`) as well as environment variables. 

**Precedence**: default < config file < environment variables < command line