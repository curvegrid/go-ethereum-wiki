# Command line options

```
NAME:
   geth - the go-ethereum command line interface

USAGE:
   geth [options] command [command options] [arguments...]
   
VERSION:
   1.4.11-stable
   
COMMANDS:
   import	import a blockchain file
   export	export blockchain into file
   upgradedb	upgrade chainblock database
   removedb	Remove blockchain and state databases
   dump		dump a specific block from storage
   monitor	Geth Monitor: node metrics monitoring and visualization
   account	manage accounts
   wallet	ethereum presale wallet
   console	Geth Console: interactive JavaScript environment
   attach	Geth Console: interactive JavaScript environment (connect to node)
   js		executes the given JavaScript files in the Geth JavaScript VM
   makedag	generate ethash dag (for testing)
   gpuinfo	gpuinfo
   gpubench	benchmark GPU
   version	print ethereum version numbers
   init		bootstraps and initialises a new genesis block (JSON)
   help, h	Shows a list of commands or help for one command
   
ETHEREUM OPTIONS:
  --datadir "/home/karalabe/.ethereum"	Data directory for the databases and keystore
  --keystore 				Directory for the keystore (default = inside the datadir)
  --networkid value			Network identifier (integer, 0=Olympic, 1=Frontier, 2=Morden) (default: 1)
  --olympic				Olympic network: pre-configured pre-release test network
  --testnet				Morden network: pre-configured test network with modified starting nonces (replay protection)
  --dev					Developer mode: pre-configured private network with several debugging flags
  --identity value			Custom node name
  --fast				Enable fast syncing through state downloads
  --lightkdf				Reduce key-derivation RAM & CPU usage at some expense of KDF strength
  --cache value				Megabytes of memory allocated to internal caching (min 16MB / database forced) (default: 128)
  --blockchainversion value		Blockchain version (integer) (default: 3)
  
ACCOUNT OPTIONS:
  --unlock value	Comma separated list of accounts to unlock
  --password value	Password file to use for non-inteactive password input
  
API AND CONSOLE OPTIONS:
  --rpc			Enable the HTTP-RPC server
  --rpcaddr value	HTTP-RPC server listening interface (default: "localhost")
  --rpcport value	HTTP-RPC server listening port (default: 8545)
  --rpcapi value	API's offered over the HTTP-RPC interface (default: "eth,net,web3")
  --ws			Enable the WS-RPC server
  --wsaddr value	WS-RPC server listening interface (default: "localhost")
  --wsport value	WS-RPC server listening port (default: 8546)
  --wsapi value		API's offered over the WS-RPC interface (default: "eth,net,web3")
  --wsorigins value	Origins from which to accept websockets requests
  --ipcdisable		Disable the IPC-RPC server
  --ipcapi value	API's offered over the IPC-RPC interface (default: "admin,debug,eth,miner,net,personal,shh,txpool,web3")
  --ipcpath "geth.ipc"	Filename for IPC socket/pipe within the datadir (explicit paths escape it)
  --rpccorsdomain value	Comma separated list of domains from which to accept cross origin requests (browser enforced)
  --jspath loadScript	JavaScript root path for loadScript and document root for `admin.httpGet` (default: ".")
  --exec value		Execute JavaScript statement (only in combination with console/attach)
  --preload value	Comma separated list of JavaScript files to preload into the console
  
NETWORKING OPTIONS:
  --bootnodes value	Comma separated enode URLs for P2P discovery bootstrap
  --port value		Network listening port (default: 30303)
  --maxpeers value	Maximum number of network peers (network disabled if set to 0) (default: 25)
  --maxpendpeers value	Maximum number of pending connection attempts (defaults used if set to 0) (default: 0)
  --nat value		NAT port mapping mechanism (any|none|upnp|pmp|extip:<IP>) (default: "any")
  --nodiscover		Disables the peer discovery mechanism (manual peer addition)
  --nodekey value	P2P node key file
  --nodekeyhex value	P2P node key as hex (for testing)
  
MINER OPTIONS:
  --mine			Enable mining
  --minerthreads value		Number of CPU threads to use for mining (default: 8)
  --minergpus value		List of GPUs to use for mining (e.g. '0,1' will use the first two GPUs found)
  --autodag			Enable automatic DAG pregeneration
  --etherbase value		Public address for block mining rewards (default = first account created) (default: "0")
  --targetgaslimit value	Target gas limit sets the artificial target gas floor for the blocks to mine (default: "4712388")
  --gasprice value		Minimal gas price to accept for mining a transactions (default: "20000000000")
  --extradata value		Block extra data set by the miner (default = client version)
  
GAS PRICE ORACLE OPTIONS:
  --gpomin value	Minimum suggested gas price (default: "20000000000")
  --gpomax value	Maximum suggested gas price (default: "500000000000")
  --gpofull value	Full block threshold for gas price calculation (%) (default: 80)
  --gpobasedown value	Suggested gas price base step down ratio (1/1000) (default: 10)
  --gpobaseup value	Suggested gas price base step up ratio (1/1000) (default: 100)
  --gpobasecf value	Suggested gas price base correction factor (%) (default: 110)
  
VIRTUAL MACHINE OPTIONS:
  --jitvm		Enable the JIT VM
  --forcejit		Force the JIT VM to take precedence
  --jitcache value	Amount of cached JIT VM programs (default: 64)
  
LOGGING AND DEBUGGING OPTIONS:
  --metrics			Enable metrics collection and reporting
  --fakepow			Disables proof-of-work verification
  --verbosity value		Logging verbosity: 0=silent, 1=error, 2=warn, 3=info, 4=core, 5=debug, 6=detail (default: 3)
  --vmodule value		Per-module verbosity: comma-separated list of <pattern>=<level> (e.g. eth/*=6,p2p=5)
  --backtrace value		Request a stack trace at a specific logging statement (e.g. "block.go:271") (default: :0)
  --pprof			Enable the pprof HTTP server
  --pprofport value		pprof HTTP server listening port (default: 6060)
  --memprofilerate value	Turn on memory profiling with the given rate (default: 524288)
  --blockprofilerate value	Turn on block profiling with the given rate (default: 0)
  --cpuprofile value		Write CPU profile to the given file
  --trace value			Write execution trace to the given file
  
EXPERIMENTAL OPTIONS:
  --shh		Enable Whisper
  --natspec	Enable NatSpec confirmation notice
  
MISCELLANEOUS OPTIONS:
  --solc value		Solidity compiler command to be used (default: "solc")
  --support-dao-fork	Updates the chain rules to support the DAO hard-fork
  --oppose-dao-fork	Updates the chain rules to oppose the DAO hard-fork
  --help, -h		show help
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