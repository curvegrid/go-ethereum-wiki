The Ethereum blockchain along with its two extension protocols Whisper and Swarm was originally conceptualized to become the supporting pillar of web3, providing the consensus, messaging and storage backbone for a new generation of distributed (actually, decentralized) applications called DApps.

The first incarnation towards this dream of web3 was a command line client providing an RPC interface into the peer-to-peer protocols. The client was soon enough extended with a web-browser-like graphical user interface, permitting developers to write DApps based on the tried and proven HTML/CSS/JS technologies.

As many DApps have more complex requirements than what a browser environment can handle, it became apparent that providing programmatic access to the web3 pillars would open the door towards a new class of applications. As such, the second incarnation of the web3 dream is to open up all our technologies for other projects as reusable components.

This was achieved starting with the 1.5 release family of `go-ethereum`, when we transitioned away from providing only a full blown Ethereum client. Instead, `go-ethereum` also started shipping official Go packages that could be embedded into third party poták, desktop and server applications. It took only a small leap from here to realize that porting our code to mobile platforms would provide an enormous value to the community.
