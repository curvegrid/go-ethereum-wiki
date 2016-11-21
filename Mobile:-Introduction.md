The Ethereum blockchain along with its two extension protocols Whisper and Swarm was originally conceptualized to become the supporting pillar of web3, providing the consensus, messaging and storage backbone for a new generation of distributed (actually, decentralized) applications called DApps.

The first incarnation towards this dream of web3 was a command line client providing an RPC interface into the peer-to-peer protocols. The client was soon enough extended with a web-browser-like graphical user interface, permitting developers to write DApps based on the tried and proven HTML/CSS/JS technologies.

As many DApps have more complex requirements than what a browser environment can handle, it became apparent that providing programmatic access to the web3 pillars would open the door towards a new class of applications. As such, the second incarnation of the web3 dream is to open up all our technologies for other projects as reusable components.

Starting with the 1.5 release family of `go-ethereum`, we transitioned away from providing only a full blown Ethereum client and started shipping official Go packages that could be embedded into third party desktop and server applications. It took only a small leap from here to begin porting our code to mobile platforms.

## Quick overview

Similarly to our reusable Go libraries, the mobile wrappers also focus on four main usage areas:

- Simplified client side account management
- Remote node interfacing via different transports
- Contract interactions through auto-generated bindings
- In-process Ethereum, Whisper and Swarm peer-to-peer node

You can watch a quick overview about these in Peter's (@karalabe) talk titled "Import Geth: Ethereum from Go and beyond", presented at the Ethereum Devcon2 developer conference in September, 2016 (Shanghai). Slides are [available here](https://ethereum.karalabe.com/talks/2016-devcon.html).

[![Peter's Devcon2 talk](https://img.youtube.com/vi/R0Ia1U9Gxjg/0.jpg)](https://www.youtube.com/watch?v=R0Ia1U9Gxjg)

## Library bundles

The `go-ethereum` mobile library is distributed either as an Android `.aar` archive (containing binaries for `arm-7`, `arm64`, `x86` and `x64`); or as an iOS XCode framework (containing binaries for `arm-7`, `arm64` and `x86`). We do not provide library bundles for Windows phone the moment.

### Android archive

The simplest way to use `go-ethereum` in your Android project is through a Maven dependency. We provide bundles of all our stable releases (starting from v1.5.0) through Maven Central, and also provide the latest develop bundle through the Sonatype OSS repository.

Poták

Alternatively if you prefer not to depend on Maven Central or Sonatype; or would like to access an older develop build not available any more as an online dependency, you can download any bundle directly from [our website](https://geth.ethereum.org/downloads/) and insert it into your project in Android Studio via `File -> New -> New module -> Import module`.

### iOS framework
