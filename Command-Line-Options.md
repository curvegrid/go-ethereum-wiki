## CLI client

    ethereum [options] [filename]

Options:

    -datadir=".ethereum": specifies the datadir to use. Takes precedence over config file.
    -export=false: export private key
    -genaddr=false: create a new priv/pub key (destructive)
    -id="": Custom client identifier
    -import="": imports the given private key (hex)
    -js=false: Start the JavaScript REPL
    -logfile="": log file (defaults to standard output)
    -maxpeer=10: maximum desired peers
    -mine=false: start dagger mining
    -port="30303": listening port
    -rpc=false: start rpc server
    -rpcport=8080: port to start json-rpc server on
    -seed=true: seed peers
    -upnp=false: enable UPnP support
    -y=false: non-interactive mode (say yes to confirmations)

filename is a 
-js        Start the JavaScript REPL
filename   Load the given file and interpret as JavaScriptdefault value after =. Note the `=` is optional.

Examples: 

executes `test.js` javascript using js API and logs to `/path/to/logfile`:

    ethereum -logfile /path/to/logfile test.js  # 
   
non-interactively imports a key and exits (used to programmatically set address from script):

    ethereum -y -import `cat mysecret.hex`

start two mining nodes with their own client ids using different data directories listening on ports 30300 and 30301 respectively:

    ethereum -upnp true -id ethersphere.0 -datadir /usr/local/share/ethereum/0 -port 30300
    ethereum -upnp true -id ethersphere.1 -datadir /usr/local/share/ethereum/1 -port 30301
    
start an rpc client on port 8000:

    ethereum -rpc true -rpcport 8000

## GUI Client 

    mist [options]
    
Options:

    -asset_path="": absolute path to GUI assets directory
    -datadir=".mist": specifies the datadir to use. Takes precedence over config file.
    -export=false: export private key
    -genaddr=false: create a new priv/pub key
    -genesis=false: prints genesis header and exits
    -id="": Custom client identifier
    -import="": imports the given private key (hex)
    -maxpeer=10: maximum desired peers
    -port="30303": listening port
    -rpc=false: start rpc server
    -rpcport=8080: port to start json-rpc server on
    -seed=true: seed peers
    -upnp=false: enable UPnP support

default value after =. Note the `=` is optional.

Example: 

    mist --asset_path /absolute/path/to/assets
