_**Note:** in this tutorial we will set up private network with 2 nodes_

## Build go-ethereum

[Build instructions](https://github.com/ethereum/go-ethereum/wiki/Building-Ethereum)

## Run command line interface

```bash
./geth --datadir="/Users/username/Library/eth0" --bootnodes="" --protocolversion="101" console
```

where

``` bash
./geth
--datadir           # the directory for storing ethereum data
--bootnodes         # it’s a private network, we don’t want to connect to any of bootnodes
--protocolversion   # let’s setup some random protocol version (optional)
console             # use geth console for convenience (optional)
```

## Find pubkey

Look at the logs, and find the following line:

```
enode://b3fafd11be8b90f77467f72087fabec7c14ae15637b2f014a11dad8a80d4be8e0d41283db7be340c181c014f2c05d5cf94b0f0743ea5e52666557c4d06f4e9e9@[::]:30303
```

`b3faf….e9e9` is our public key, copy it. We will need it later

## Run second instance of geth cli

```bash
./geth --datadir="/Users/username/Library/eth1" --bootnodes="enode://b3fafd11be8b90f77467f72087fabec7c14ae15637b2f014a11dad8a80d4be8e0d41283db7be340c181c014f2c05d5cf94b0f0743ea5e52666557c4d06f4e9e9@127.0.0.1:30303" --port="30302" --protocolversion="101" console
```

where

```bash
./geth
--bootnodes            # is in format: pubkey@ip:port
--port                 # we need to run this node on different port than previous one
```

## That's it!

Your private network is working!

You can test it by typing in geth console:

```javascript
net
```

You should see the following output:

```javascript
{
  listening: true,
  peerCount: 1
}
```
