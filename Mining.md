* [So what is mining anyway?](https://github.com/ethereum/wiki/wiki/Mining#so-what-is-mining-anyway) _(main wiki)_
* [Mining with geth](#miningwithgeth)
* [GPU mining](#gpumining)
* [Resources](#resources)

## Mining with Geth

When you start up your ethereum node with `geth` it is not mining by default. To start it in mining mode, you use the `-mine` [command line option](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options). The `-minerthreads` parameter can be used to set the number parallel mining threads (defaulting to the total number of processor cores). 

     geth -mine -minerthreads=4

You can also start and stop mining at runtime using the [console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminerstart). 

```
> admin.miner.start()
true
> admin.miner.stop()
true
```

In order to earn ether through you need to have a **coinbase** (or **etherbase**) address set. This etherbase defaults to your [primary account](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts). Note that miner will not complain if you got no etherbase address set, however money then will be sent to a zero address meaning you are not gonna receive the mining reward. 

```
> eth.coinbase
'0x'
> admin.newAccount()
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
'ffd25e388bf07765e6d7a00d6ae83fa750460c7e'
> eth.coinbase
'0xffd25e388bf07765e6d7a00d6ae83fa750460c7e'
```

Note that your coinbase does not need to be an address of a local account, just an existing one. 

```
eth.accounts
[]
eth.coinbase = 'a4d8e9cae4d04b093aac82e6cd355b6b963fb7ff'
```

There is an option [to add extra Data](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminersetextra) (up to the limit of 1Kb) to your mined blocks. By convention this is interpreted as a unicode string, so you can set your vanity tag.

```
admin.miner.setExtra("ΞTHΞЯSPHΞЯΞ")
...
admin.debug.printBlock(131805)
BLOCK(be465b020fdbedc4063756f0912b5a89bbb4735bd1d1df84363e05ade0195cb1): Size: 531.00 B TD: 643485290485 {
NoNonce: ee48752c3a0bfe3d85339451a5f3f411c21c8170353e450985e1faab0a9ac4cc
Header:
[
...
        Coinbase:           a4d8e9cae4d04b093aac82e6cd355b6b963fb7ff
        Number:             131805
        Extra:              ΞTHΞЯSPHΞЯΞ
...
}
```

You can check your hashrate with [admin.miner.hashrate](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminerhashrate) , the result is in KH/s (1000 Hash operations per second). 

```
> admin.miner.hashrate()
712
```

After you successfully mined some blocks, you can check the ether balance of your coinbase account. Now assuming your coinbase is a local account:

```
> eth.getBalance(eth.coinbase).toNumber();
'34698870000000' 
```

In order to spend your earnings [on gas to transact](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions), you will need to have this account unlocked. 

```
> admin.unlock(eth.coinbase)
Please enter a passphrase now.
Passphrase:
true
```

## GPU mining

**TODO**
Using a graphic card processor chip for mining.

## Resources:

* https://blog.ethereum.org/2014/07/05/stake/
* https://blog.ethereum.org/2014/10/03/slasher-ghost-developments-proof-stake/
* https://blog.ethereum.org/2014/06/19/mining/
* https://github.com/ethereum/wiki/wiki/Dagger-Hashimoto
* https://github.com/ethereum/wiki/wiki/Ethash

