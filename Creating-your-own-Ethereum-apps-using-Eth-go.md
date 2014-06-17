The modular nature of Go and the Ethereum Go implementation, [eth-go](github.com/ethereum/eth-go), make it very easy to build your own Ethereum based applications. 

This post will show you the minimal steps required to build your own Ethereum based application.

The first thing you will have to do is pick a data folder and tell EthUtil to use said folder for all files and configs.

```go
func main() {
	ethutil.ReadConfig(".test", ethutil.LogStd, nil, "MyEthApp")
}
```  

ReadConfig takes four arguments. The data folder to use, a log flag, a globalConf instance and an id string to identify your app to other nodes in the network.

Next it's time to create our actual Ethereum object.

```go
func main() {
        ethutil.ReadConfig(".test", ethutil.LogStd, nil, "MyEthApp")
        ethereum, err := eth.New(eth.CapDefault, false)
        if err != nil {
            panic(fmt.Sprintf("Could not start node: %s\n", err))
        }
        ethereum.Port = "10101"
        ethereum.MaxPeers = 10
}
```

New requires two arguments; the capabilities of the node and whether or not to use UPNP for port-forwarding. If you don't want to fallback to client-only features set an Ethereum port and the max amount of peers this node can connect to. 

Now in order to identify itself to other nodes your node will require a public/private keypair. The easiest way to generate this is via the utils package. Let's add that in.

```go
func main() {
	ethutil.ReadConfig(".test", ethutil.LogStd, nil, "MyEthApp")

	ethereum, err := eth.New(eth.CapDefault, false)
	if err != nil {
		panic(fmt.Sprintf("Could not start node: %s\n", err))
	}

	utils.CreateKeyPair(false)

	ethereum.Port = "10101"
	ethereum.MaxPeers = 10
}
```

The only thing left now is to start it and to keep running our node until an exit signal comes in. The complete program will now look something like this.

```go
package main

import (
	"github.com/ethereum/eth-go"
	"github.com/ethereum/eth-go/ethutil"
)

func main() {
	ethutil.ReadConfig(".test", ethutil.LogStd, nil, "MyEthApp")

	ethereum, err := eth.New(eth.CapDefault, false)
	if err != nil {
		panic(fmt.Sprintf("Could not start node: %s\n", err))
	}
	utils.CreateKeyPair(false)

	ethereum.Port = "10101"
	ethereum.MaxPeers = 10

	ethereum.Start(true)
	ethereum.WaitForShutdown()
}

```

`ethereum.Start()` takes one argument, whether or not we want to connect to one of the known seed nodes. If you want your own little testnet-in-a-box you can disable it else set it to true.

Your node should now be catching up with the blockchain. From here on out you are own your own. You could create a reactor to listen to specific events or just dive into the chain state directly. If you want to look at some example code you can check [DNSEth here.](https://github.com/maran/dnseth)

Have fun!