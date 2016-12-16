```
$ geth help
NAME:
   geth - the go-ethereum command line interface

   Copyright 2013-2016 The go-ethereum Authors
                                             
USAGE:
   geth [options] command [command options] [arguments...]
   
VERSION:
   1.5.5-unstable
   
COMMANDS:
   init       Bootstrap and initialize a new genesis block
   import     Import a blockchain file
   export     Export blockchain into file
   upgradedb  Upgrade chainblock database
   removedb   Remove blockchain and state databases
   dump       Dump a specific block from storage
   monitor    Monitor and visualize node metrics
   account    Manage accounts
   wallet     Manage Ethereum presale wallets
   console    Start an interactive JavaScript environment
   attach     Start an interactive JavaScript environment (connect to node)
   js         Execute the specified JavaScript files
   makedag    Generate ethash DAG (for testing)
   version    Print version numbers
   license    Display license information
   help, h    Shows a list of commands or help for one command
   
ETHEREUM OPTIONS:
  --datadir "/home/tron/.ethereum"  Data directory for the databases and keystore
  --keystore                        Directory for the keystore (default = inside the datadir)
  --networkid value                 Network identifier (integer, 0=Olympic (disused), 1=Frontier, 2=Morden (disused), 3=Ropsten) (default: 1)
  --olympic                         Olympic network: pre-configured pre-release test network
  --testnet                         Ropsten network: pre-configured test network
  --dev                             Developer mode: pre-configured private network with several debugging flags
  --identity value                  Custom node name
  --fast                            Enable fast syncing through state downloads
  --light                           Enable light client mode
  --lightserv value                 Maximum percentage of time allowed for serving LES requests (0-90) (default: 0)
  --lightpeers value                Maximum number of LES client peers (default: 20)
  --lightkdf                        Reduce key-derivation RAM & CPU usage at some expense of KDF strength
  
PERFORMANCE TUNING OPTIONS:
  --cache value            Megabytes of memory allocated to internal caching (min 16MB / database forced) (default: 128)
  --trie-cache-gens value  Number of trie node generations to keep in memory (default: 120)
  
                                                                                                                  
ACCOUNT OPTIONS:
  --unlock value    Comma separated list of accounts to unlock
  --password value  Password file to use for non-inteactive password input
  
API AND CONSOLE OPTIONS:
  --rpc                  Enable the HTTP-RPC server
  --rpcaddr value        HTTP-RPC server listening interface (default: "localhost")
  --rpcport value        HTTP-RPC server listening port (default: 8545)
  --rpcapi value         API's offered over the HTTP-RPC interface (default: "eth,net,web3")
  --ws                   Enable the WS-RPC server
  --wsaddr value         WS-RPC server listening interface (default: "localhost")
  --wsport value         WS-RPC server listening port (default: 8546)
  --wsapi value          API's offered over the WS-RPC interface (default: "eth,net,web3")
  --wsorigins value      Origins from which to accept websockets requests
  --ipcdisable           Disable the IPC-RPC server
  --ipcapi value         APIs offered over the IPC-RPC interface (default: "admin,debug,eth,miner,net,personal,shh,txpool,web3")
  --ipcpath "geth.ipc"   Filename for IPC socket/pipe within the datadir (explicit paths escape it)
  --rpccorsdomain value  Comma separated list of domains from which to accept cross origin requests (browser enforced)
  --jspath loadScript    JavaScript root path for loadScript (default: ".")
  --exec value           Execute JavaScript statement (only in combination with console/attach)
  --preload value        Comma separated list of JavaScript files to preload into the console
  
NETWORKING OPTIONS:
  --bootnodes value     Comma separated enode URLs for P2P discovery bootstrap
  --port value          Network listening port (default: 30303)
  --maxpeers value      Maximum number of network peers (network disabled if set to 0) (default: 25)
  --maxpendpeers value  Maximum number of pending connection attempts (defaults used if set to 0) (default: 0)
  --nat value           NAT port mapping mechanism (any|none|upnp|pmp|extip:<IP>) (default: "any")
  --nodiscover          Disables the peer discovery mechanism (manual peer addition)
  --v5disc              Enables the experimental RLPx V5 (Topic Discovery) mechanism
  --nodekey value       P2P node key file
  --nodekeyhex value    P2P node key as hex (for testing)
  
MINER OPTIONS:
  --mine                  Enable mining
  --minerthreads value    Number of CPU threads to use for mining (default: 8)
  --autodag               Enable automatic DAG pregeneration
  --etherbase value       Public address for block mining rewards (default = first account created) (default: "0")
  --targetgaslimit value  Target gas limit sets the artificial target gas floor for the blocks to mine (default: "4712388")
  --gasprice value        Minimal gas price to accept for mining a transactions (default: "20000000000")
  --extradata value       Block extra data set by the miner (default = client version)
  
GAS PRICE ORACLE OPTIONS:
  --gpomin value       Minimum suggested gas price (default: "20000000000")
  --gpomax value       Maximum suggested gas price (default: "500000000000")
  --gpofull value      Full block threshold for gas price calculation (%) (default: 80)
  --gpobasedown value  Suggested gas price base step down ratio (1/1000) (default: 10)
  --gpobaseup value    Suggested gas price base step up ratio (1/1000) (default: 100)
  --gpobasecf value    Suggested gas price base correction factor (%) (default: 110)
  
VIRTUAL MACHINE OPTIONS:
  --jitvm           Enable the JIT VM
  --forcejit        Force the JIT VM to take precedence
  --jitcache value  Amount of cached JIT VM programs (default: 64)
  
LOGGING AND DEBUGGING OPTIONS:
  --ethstats value          Reporting URL of a ethstats service (nodename:secret@host:port)
  --metrics                 Enable metrics collection and reporting
  --fakepow                 Disables proof-of-work verification
  --verbosity value         Logging verbosity: 0=silent, 1=error, 2=warn, 3=info, 4=core, 5=debug, 6=detail (default: 3)
  --vmodule value           Per-module verbosity: comma-separated list of <pattern>=<level> (e.g. eth/*=6,p2p=5)
  --backtrace value         Request a stack trace at a specific logging statement (e.g. "block.go:271") (default: :0)
  --pprof                   Enable the pprof HTTP server
  --pprofaddr value         pprof HTTP server listening interface (default: "127.0.0.1")
  --pprofport value         pprof HTTP server listening port (default: 6060)
  --memprofilerate value    Turn on memory profiling with the given rate (default: 524288)
  --blockprofilerate value  Turn on block profiling with the given rate (default: 0)
  --cpuprofile value        Write CPU profile to the given file
  --trace value             Write execution trace to the given file
  
EXPERIMENTAL OPTIONS:
  --shh      Enable Whisper
  --natspec  Enable NatSpec confirmation notice
  
MISCELLANEOUS OPTIONS:
  --solc value         Solidity compiler command to be used (default: "solc")
  --netrestrict value  Restricts network communication to the given IP networks (CIDR masks)
  --help, -h           show help
  
```