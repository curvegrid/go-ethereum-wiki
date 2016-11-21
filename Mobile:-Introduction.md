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