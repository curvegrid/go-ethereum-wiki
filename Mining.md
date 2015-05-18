* [So what is mining anyway?](https://github.com/ethereum/wiki/wiki/Mining#so-what-is-mining-anyway) _(main wiki)_

# Mining with Geth

_**NOTE:** Ensure your blockchain is fully synchronised with the main chain before starting to mine, otherwise you will not be mining on the main chain._

When you start up your ethereum node with `geth` it is not mining by default. To start it in mining mode, you use the `--mine` [command line option](https://github.com/ethereum/go-ethereum/wiki/Command-Line-Options). The `-minerthreads` parameter can be used to set the number parallel mining threads (defaulting to the total number of processor cores). 

`geth --mine --minerthreads=4`

You can also start and stop mining at runtime using the [console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminerstart). 

```
> admin.miner.start()
true
> admin.miner.stop()
true
```

In order to earn ether through you need to have a **coinbase** (or **etherbase**) address set. This etherbase defaults to your [primary account](https://github.com/ethereum/go-ethereum/wiki/Managing-your-accounts). If you got no etherbase address set, then `geth --mine` will not start up.

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
geth --etherbase '0xa4d8e9cae4d04b093aac82e6cd355b6b963fb7ff' --mine console 2>>geth.log
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

You can check your hashrate with [admin.miner.hashrate](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console#adminminerhashrate) , the result is in H/s (Hash operations per second). 

```
> admin.miner.hashrate()
712000
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

You can check which blocks are mined by a particular miner (address) with the following code snippet on the console:

```
function minedBlocks(lastn, addr) {
  addrs = [];
  if (!addr) {
    addr = eth.coinbase
    limit = eth.blockNumber - lastn
    for (i = eth.blockNumber; i >= limit; i--) {
	if (eth.getBlock(i).miner == addr) {
	  addrs.push(i)
        }
    }
    return addrs
}
// scans the last 1000 blocks and returns the blocknumbers of blocks mined by your coinbase 
// (more precisely blocks the mining reward for which is sent to your coinbase).   
minedBlocks(1000, eth.coinbase);
//[352708, 352655, 352559]
```

Mining success depends on the set block difficulty. Block difficulty dynamically adjusts each block in order to regulate the network hashing power to produce a 12 second blocktime. Your chances of finding a block therefore follows from your hashrate relative to difficulty. The time you need to wait you are expected to find a block can be estimated with the following code:

**INCORRECT...CHECKING**
```
etm = eth.getBlock("latest").difficulty/admin.miner.hashrate(); // estimated time in seconds
Math.floor(etm / 3600.) + "h " + Math.floor((etm % 3600)/60) + "m " +  Math.floor(etm % 60) + "s";
// 1h 3m 30s
```

Given a difficulty of 3 billion, a typical CPU with 800KH/s is expected to find a block every 

Note that it will happen often that you find a block yet it never makes it to the canonical chain. This means when you locally include your mined block, the current state will show the mining reward credited to your account, however, after a while, the better chain is discovered and we switch to a chain in which your block is not included and therefore no mining reward is credited. Therefore it is quite possible that as a miner monitoring their coinbase balance will find that it may fluctuate quite a bit. 

## Mining Rewards

Note that mining 'real' Ether will start with the Frontier release. On the Olympics testnet, the [Frontier pre-release](http://ethereum.gitbooks.io/frontier-guide/), the ether mined have no value.


### Transaction fees

### Uncles

## Ethash DAG

Ethash uses a **DAG** (directed acyclic graph) for the proof of work algorithm, this is generated for each **epoch**, i.e every 30000 blocks (100 hours). The DAG takes a long time to generate. Since the clients only generate it on demand, you may see a long wait at each epoch transition before the first block of the new epoch is found. However, the DAG only depends on block number, so it CAN and SHOULD be calculated in advance to avoid long wait at each epoch transition. 

```
geth makedag <block number> <outputdir>
```

For instance `geth makedag 360000 ~/.ethash`. Note that ethash uses `~/.ethash` (Mac/Linux) or `~/AppData/Ethash`  (Windows) for the DAG so that it can shared between clients. 

# GPU mining

***

## Hardware

The algorithm is memory hard and in order to fit the DAG into memory, it needs 1-2GB of RAM on each GPU. If you get ` Error GPU mining. GPU memory fragmentation?` you havent got enough memory.

The GPU miner is implemented in OpenCL, so AMD GPUs will be 'faster' than same-category NVIDIA GPUs.
`fglrx` a `cudacl` are your friends.


ASICs and FPGAs are relatively inefficient and therefore discouraged. 

Getting openCL for your chipset and platform, try:
* [AMD SDK openCL](http://developer.amd.com/tools-and-sdks/opencl-zone/amd-accelerated-parallel-processing-app-sdk)
* [NVIDIA CUDA openCL](https://developer.nvidia.com/cuda-downloads)

## On Ubuntu
```
apt-get install fglrx
// wget, tar, opencl
aticonfig --adapter=all --initial
```

## On MacOSx

```
wget http://developer.download.nvidia.com/compute/cuda/7_0/Prod/local_installers/cuda_7.0.29_mac.pkg
sudo installer -pkg ~/Desktop/cuda_7.0.29_mac.pkg -target /
brew update
brew tap ethereum/ethereum
brew reinstall cpp-ethereum --with-gpu-mining --devel --headless --build-from-source
```

You check your cooling status:

    aticonfig --adapter=0 --od-gettemperature

## Mining Software

Currently `geth` only supports a CPU miner natively. We are working on a GPU miner, but it may not be available for the Frontier release. Geth however can be used in conjunction with `ethminer`, using the standalone miner as workers and `geth` as scheduler communicating via [JSON-RPC](https://github.com/ethereum/wiki/wiki/JSON-RPC). 

The [C++ implementation of Ethereum](https://github.com/ethereum/cpp-ethereum/) (not officially released) however has a GPU miner. It can be used from `eth`, `AlethZero` (GUI) and `ethMiner` (the standalone miner). 

[You can install this](https://github.com/ethereum/cpp-ethereum/wiki/Installing-clients) via ppa on linux, brew tap on MacOS or from source. 

On MacOS:
```
brew install cpp-ethereum --with-gpu-mining --devel --build-from-source
```

On Linux:
```
apt-get install cpp-ethereum 
```

On Windows: 

## GPU mining with ethminer 
To mine with `eth`:

```
eth -m on -G -a <coinbase> -i -v 8 //
```

To install `ethminer` from source:

```
cd cpp-ethereum
cmake -DETHASHCL=1 -DGUI=0
make -j4
make install
```

To set up GPU mining you need a coinbase account. It can be an account created locally or remotely. 

### Using ethminer with geth

```
geth account new
geth --rpccorsdomain localhost 2>> geth.log &
ethminer -G  // -G for GPU, -M for benchmark
tail -f geth.log
```

`ethminer` communicates with geth on port 8545 (the default RPC port in geth). You can change this by giving the [`--rpcport` option](https://github.com/ethereum/go-ethereum/Command-Line-Options) to `geth`.
Ethminer will find get on any port. Note that you need to set the CORS header with `--rpccorsdomain localhost`. 

Also note that you do **not** need to give `geth` the `--mine` option or start the miner in the console unless you want to do CPU mining on TOP of GPU mining. 

If the default for `ethminer` does not work try to specify the OpenCL device with: `--opencl-device X` where X is 0, 1, 2, etc.
When running `ethminer` with `-M` (benchmark), you should see something like:

    Benchmarking on platform: { "platform": "NVIDIA CUDA", "device": "GeForce GTX 750 Ti", "version": "OpenCL 1.1 CUDA" }

or 

    Benchmarking on platform: { "platform": "Apple", "device": "Intel(R) Xeon(R) CPU E5-1620 v2 @ 3.70GHz", "version": "OpenCL 1.2 " }

To debug `geth`:

```
geth  --rpccorsdomain "localhost" --verbosity 6 2>> geth.log
```

To debug the miner: 

```
make -DCMAKE_BUILD_TYPE=Debug -DETHASHCL=1 -DGUI=0
gdb --args ethminer -G -M
```

### ethminer and eth

`ethminer` can be used in conjunction with `eth` via rpc

```
eth -i -v 8 -j // -j for rpc
ethminer -G -M // -G for GPU, -M for benchmark
tail -f geth.log
```

or you can use `eth` to GPU mine by itself:

```
eth -m on -G -a <coinbase> -i -v 8 //
```

# Further Resources:

* [ethereum forum mining FAQ live update](https://forum.ethereum.org/discussion/197/mining-faq-live-updates)
* [yates randall mining video](https://www.youtube.com/watch?v=CnKnclkkbKg)
* https://blog.ethereum.org/2014/07/05/stake/
* https://blog.ethereum.org/2014/10/03/slasher-ghost-developments-proof-stake/
* https://blog.ethereum.org/2014/06/19/mining/
* https://github.com/ethereum/wiki/wiki/Ethash
* [Benchmarking results for GPU mining](https://forum.ethereum.org/discussion/2134/gpu-mining-is-out-come-and-let-us-know-of-your-bench-scores)
* [historic moment](https://twitter.com/gavofyork/status/586623875577937922)
* [live mining statistic](https://etherapps.info/stats/mining)
* [netstat ethereum network monitor](stats.ethdev.com)
