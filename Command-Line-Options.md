# Command line options

```
NAME:
   geth - the go-ethereum command line interface

USAGE:
   geth [options] command [command options] [arguments...]
   
VERSION:
   1.4.0
   
COMMANDS:
   blocktest    loads a block test file
   import       import a blockchain file
   export       export blockchain into file
   upgradedb    upgrade chainblock database
   removedb     Remove blockchain and state databases
   dump         dump a specific block from storage
   monitor      Geth Monitor: node metrics monitoring and visualization
   account      manage accounts
   wallet       ethereum presale wallet
   makedag      generate ethash dag (for testing)
   gpuinfo      gpuinfo
   gpubench     benchmark GPU
   version      print ethereum version numbers
   init         bootstraps and initialises a new genesis block (JSON)
   console      Geth Console: interactive JavaScript environment
   attach       Geth Console: interactive JavaScript environment (connect to node)
   js           executes the given JavaScript files in the Geth JavaScript VM
   help, h      Shows a list of commands or help for one command
   
ETHEREUM OPTIONS:
  --datadir "/home/youruser/.ethereum"  Data directory for the databases and keystore
  --keystore                            Directory for the keystore (default = inside the datadir)
  --networkid "1"                       Network identifier (integer, 0=Olympic, 1=Frontier, 2=Morden)
  --olympic                             Olympic network: pre-configured pre-release test network
  --testnet                             Morden network: pre-configured test network with modified starting nonces (replay protection)
  --dev                                 Developer mode: pre-configured private network with several debugging flags
  --genesis                             Insert/overwrite the genesis block (JSON format)
  --identity                            Custom node name
  --fast                                Enables fast syncing through state downloads
  --lightkdf				Reduce key-derivation RAM & CPU usage at some expense of KDF strength
  --cache "0"                           Megabytes of memory allocated to internal caching (min 16MB / database forced)
  --blockchainversion "3"               Blockchain version (integer)
  
ACCOUNT OPTIONS:
  --unlock      Unlock an account (may be creation index) until this program exits (prompts for password)
  --password    Password file to use with options/subcommands needing a pass phrase
  
API AND CONSOLE OPTIONS:
  --rpc                                                                 Enable the HTTP-RPC server
  --rpcaddr "localhost"                                                 HTTP-RPC server listening interface
  --rpcport "8545"                                                      HTTP-RPC server listening port
  --rpcapi "eth,net,web3"                                               API's offered over the HTTP-RPC interface
  --rpccorsdomain                                                       Domains from which to accept cross origin requests (browser enforced)
  --ws                                                                  Enable the weboscket RPC server
  --wsaddr "localhost"                                                  WS-RPC server listening interface
  --wsport "8546"                                                       WS-RPC server listening port
  --wsapi "eth,net,web3"                                                API's offered over the WS-RPC interface
  --wsorigins                                                           Origins from which to accept websockets requests
  --ipcdisable                                                          Disable the IPC-RPC server
  --ipcapi "admin,db,eth,debug,miner,net,shh,txpool,personal,web3"      API's offered over the IPC-RPC interface
  --ipcpath "/home/youruser/.ethereum/geth.ipc"                         Filename for IPC socket/pipe
  --jspath "."                                                          JavaSript root path for `loadScript` and document root for `admin.httpGet`
  --exec                                                                Execute JavaScript statement (only in combination with console/attach)
  --preload                                                             Comma separated list of JavaScript files to preload into the console

NETWORKING OPTIONS:
  --bootnodes           Space-separated enode URLs for P2P discovery bootstrap
  --port "30303"        Network listening port
  --maxpeers "25"       Maximum number of network peers (network disabled if set to 0)
  --maxpendpeers "0"    Maximum number of pending connection attempts (defaults used if set to 0)
  --nat "any"           NAT port mapping mechanism (any|none|upnp|pmp|extip:<IP>)
  --nodiscover          Disables the peer discovery mechanism (manual peer addition)
  --nodekey             P2P node key file
  --nodekeyhex          P2P node key as hex (for testing)
  
MINER OPTIONS:
  --mine                        Enable mining
  --minerthreads "8"            Number of CPU threads to use for mining
  --minergpus                   List of GPUs to use for mining (e.g. '0,1' will use the first two GPUs found)
  --autodag                     Enable automatic DAG pregeneration
  --etherbase "0"               Public address for block mining rewards (default = first account created)
  --targetgaslimit "4712388"	Target gas limit sets the artificial target gas floor for the blocks to mine
  --gasprice "20000000000"      Minimal gas price to accept for mining a transactions
  --extradata                   Block extra data set by the miner (default = client version)
  
GAS PRICE ORACLE OPTIONS:
  --gpomin "20000000000"        Minimum suggested gas price
  --gpomax "500000000000"       Maximum suggested gas price
  --gpofull "80"                Full block threshold for gas price calculation (%)
  --gpobasedown "10"            Suggested gas price base step down ratio (1/1000)
  --gpobaseup "100"             Suggested gas price base step up ratio (1/1000)
  --gpobasecf "110"             Suggested gas price base correction factor (%)
  
VIRTUAL MACHINE OPTIONS:
  --jitvm               Enable the JIT VM
  --forcejit            Force the JIT VM to take precedence
  --jitcache "64"       Amount of cached JIT VM programs
  
LOGGING AND DEBUGGING OPTIONS:
  --metrics			Enable metrics collection and reporting
  --verbosity "3"               Logging verbosity: 0-6 (0=silent, 1=error, 2=warn, 3=info, 4=core, 5=debug, 6=debug detail)
  --vmodule ""                  Per-module verbosity: comma-separated list of <module>=<level>, where <module> is file literal or a glog pattern
  --backtrace ":0"              Request a stack trace at a specific logging statement (e.g. "block.go:271")
  --pprof                       Enable the profiling server on localhost
  --pprofport "6060"            Profile server listening port
  --memprofilerate "524288"	Turn on memory profiling with the given rate
  --blockprofilerate "0"	Turn on block profiling with the given rate
  --cpuprofile 			Write CPU profile to the given file
  --trace 			Write execution trace to the given file
  
EXPERIMENTAL OPTIONS:
  --shh         Enable Whisper
  --natspec     Enable NatSpec confirmation notice
  
MISCELLANEOUS OPTIONS:
  --solc "solc" Solidity compiler command to be used
  --help, -h    show help
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

    geth --verbosity 5 --jspath /mydapp/js console 2>> /path/to/logfile

Execute `test.js` javascript using js API and log Debug-level messages to `/path/to/logfile`:

    geth --verbosity 6 js test.js  2>> /path/to/logfile

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

    geth --rpc --rpcport 8000 --rpccorsdomain "*"

Launch the client without network:

    geth --maxpeers 0 --nodiscover --networdid 3301 js justwannarunthis.js

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

### Attach
Attach a console to a running geth instance. By default this happens over IPC on the default IPC endpoint but when necessary a custom endpoint could be specified:

```
geth attach                   # connect over IPC on default endpoint
geth attach ipc:/some/path    # connect over IPC on custom endpoint
geth attach http://host:8545  # connect over HTTP
geth attach ws://host:8546    # connect over websocket
```

### Init
With the init command it is possible to create a chain with a custom genesis block and chain configuration (currently only the homestead transition block can be configured). It respects the `--datadir` argument and accepts a JSON file describing the chain configuration. If you omit the `--datadir` flag the genesis block will be written in the default datadir. If there is a synced chain in the default datadir it will be destroyed.

```
geth --datadir <some/location/where/to/create/chain> init genesis.json
```

Example genesis.json file:
```
{
   "coinbase": "0x0000000000000000000000000000000000000000",
   "config": {
      "homesteadBlock": 5
   },
   "difficulty": "0x20000",
   "extraData": "0x",
   "gasLimit": "0x2FEFD8",
   "mixhash": "0x00000000000000000000000000000000000000647572616c65787365646c6578",
   "nonce": "0x0",
   "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
   "timestamp": "0x00",
   "alloc": {
      "dbdbdb2cbd23b783741e8d7fcf51e459b497e4a6":{
         "balance":"100000000000000000000000000000"
      },
      "e6716f9544a56c530d868e4bfbacb172315bdead":{
         "balance":"100000000000000000000000000000"
      }
   }
}
```

To use the created chain start geth with:
```
geth --datadir <some/location/where/to/create/chain>
```