# SWARM -- IPFS

The question about the similarities/differences between Ethereum's swarm and IPFS and their future plans including 
various levels of collaboration comes up very regularly. I will try to do justice to this great interest by giving
a rather detailed answer (I imagine with time writing this up in even more technical detail using my scattered 
unedited notes).

**Disclaimer**: being the main author of swarm, this note is inevitably *biased*. 
hopefully not so much in valuation, but certainly in representation. Also this reflects my 
my understanding of IPFS, which is incomplete or may well be incorrect

**tl;dr**
 Swarm and IPFS 
 * are "same same but different" - the big picture and high level vision connects the two projects under various banners 
   of the next webz 
 * the 2 projects are close allies and open to collaboration both on technical and marketing fronts
 * In order to push our highly overlapping agenda, and most effectively benefit from each other's resources, 
   various degrees of integration of the actual technologies are proposed 

in this note, after summarising the strikingly aligned motivational context and high-level similarities between 
the two projects, I turn to discussing the differences covering organisational, ideological and technological aspects.
Finally, I propose multiple potential avenues for collaboration and technology integration spelling out pros and cons of each.

## similarities

**tl;dr** swarm and IPFS both offer comprehensive solutions for efficient decentralised storage layer for the next generation internet.
both high level goals and technology used are very similar.

Both swarm and IPFS systems aspire to provide 
* a generic decentralised distributed storage solution
* content delivery protocol

This is carried out by creating a network of cooperating nodes each running a client conforming to a rigorously defined
communication protocol for storage and retrieval of arbitrary content.
Leveraging individual participant's surplus storage and bandwidth, the network nodes collectively provide 
a serverless hosting platform.

both projects

* aspire to offer a layer of (monetary) incentivisation for participating nodes to encourage 
  healthy operations and/or insurance/reassurance :) to users as well as compensation for resource use.
* use some sort of block storage model where larger documents are chopped up and the pieces can fetch parallelly
* provide integrity protection by content addressing (also for encrypted partial content)
* both offer url schemes and decentralised domain name resolution 
* transparent and efficient mapping of file system directories to sets of storage objects

As a result both are in principle ideally suited for replacing the data layer of the current broken internet 
and serve as storage layers for the web3 vision (alongside other kin ventures, notably zeronet, Maidsafe, i2p, storj, etc.)
with the usual must-have properties of distributed document storage 

* low-latency retrieval
* efficient auto-scaling (content caching)
* reliable, fault-tolerant operation, resistent to node's disconnections, intermittent availability
* zero-downtime
* censorship-resistant
* potentially permanent versioned archive of content

## Differences 

**tl;dr**
Subtle but important differences in their design will likely keep the two projects steady and separate on their own track. 

Since the big picture and the high level solution are so magically aligned, the differences are elsewhere. 
I group them under (A) development status/popularity/user base (B) philosophical/ethical/political 
(C) lower-level technicalities.

### (A) status

**tl;dr**: IPFS is quite a bit ahead in code maturity, scaling, adoption, community engagement and dedicated developers. 
Yet swarm's place in the ethereum ecosystem translates to inherent infrastructural advantage. 

Advice: if you need a reliable near production-level distributed CDN NOW, choose IPFS and run.

* both IPFS and swarm are fully open source and the reference implementations are written in the Go language (swarm has an outdated java version, IPFS has javasctipt)
* both IPFS and swarm are alpha software before their production release
* IPFS has been proven to scale quite reasonably, swarm is just starting to be tested on larger scale (though swarm is built on top of devp2p, the ethereum p2p networking layer which itself barely need testing)
* IPFS has had the product open for longer, and recruited a decent userbase, Swarm has not really come out yet, the POC release series just started this year
* IPFS has a lot of material out, videos, good docs, papers. Swarm has 2 devcontalks, scattered docs and 2 papers (first 2 in the ethersphere orange paper series) about the incentives to be published mid April! And a swarm guide is in the making
* IPFS has a working network (no incentivisation), Swarm just recently launched the first stage of developer testnet (soon to be public)
* IPFS already serves as a working solution for real-world businesses and is supported by an enthusiastic user-base
* swarm benefits from strong synergy with ethereum, its ever more successful ecosystem, live network of users and its organisational background in the form of reliable continued funding from the non-profit foundation. IPFS also has reliable funding sources and also used and supported by members of the ethereum commnity.

Despite strong voices from the community disapproving of reinventing the wheel, swarm as
a comprenensive in-house solution suffered and survived the toughest of times: the austerity measures of last autumn due to the financial difficulties of the foundation slowed the development. The current favourable circumstances make our original vision realistic once again and development has seen a new surge likely further testified by expanding the dev team. 
Admittedly biased, but i am convinced that building our own hand taylored system is a winning ticket
to enable such a pivotal component of web3 to quickly and flexibly adapt and coevolve with Ethereum (EVM), 
its governance and funding aligned with that of Ethereum.

It is crucial that swarm's privileged infrastructural/organisational status  should not be by itself the deciding factor 
in the predominant adoption among available alternatives when web3 comes to the masses. 
My intention is that users' choice be based on inherent merits of the particular technology and that 
the selection is not unduely restricted by arbitrary choices/limitations of ethereum 
(e.g., use of devp2p network layer, see below). 

Conversely, by bringing more and more discussions about our roadmap to the public, we aspire to counterbalance IPFS's 
advantage due to longer time being around. Maturity has a rightful place in choosing a technology if you need it now, so 
the discussion here is relevant to developers with medium-to-long-term plans. Hopefully by the time both projects are 
out with production-ready release candidates, the differences in this section become insignificant to let features dominate 
the evaluation.

### (B) philosophical

**tl;dr**: no critical mismatch but different enough to predict and justify parallel evolution of the 2 projects.

**Advice** if you have at most moderate concern for censorship or live in areas of the world where the cartels 
of information copyrighting or advocates of restricted freedom of information have the resources to come after you, 
you better off chosing IPFS. If the cause of total transparency and unimpeded support for freedom of information is
important to you (be it on moral or opportunistic grounds), then consider supporting swarm. 

**Advice** if you are in no way married to ethereum blockchain or commited to uniformly incentivised and insured storage, 
you may not find the swarm's offering distinctly superior to IPFS or other existing alternatives.

Swarm is very specifically meant to be part of the ethereum ecosystem. From the outset, it was always conceived of as 
one of the three pillars of the next webz, and alongside Ethereum and Whisper define the holy trinity of web3 components.
Its development is guided and inspired by Ethereum's needs (most importantly the need of hosting dapps, 
contract source/metadata and the blockchain/state/etc). It is developed in the context of ethereum's capabilities 
(including potential limitations) and as long as funded by the foundation is guaranteed to cater for 
specific uses arising in the ethereum ecosystem.

Meanwhile IPFS is a unifying solution catering for integrating many existing protocols.

Swarm has a very strong anticensorship stance. It incentivises content agnostic collective storage 
(block propagation/distribution scheme). 
Implements plausible deniability with implausible accountability through double masking (not currently done). 
IPFS believes that wider adoption warrants compromising on censorship by providing tools for blacklisting, 
source-filtering though using these is entirely voluntary.

### (C) technicalities

**tl;dr**:
* swarm's core storage component as an immutable content addressed chunkstore rather than a generic DHT. 
* the two systems use different network communications layer and peer management protocol 
* swarm has deep integration with the Ethereum blockchain and the incentive system benefits from 
  both smart contracts as well as the semi-stable peerpool. Filecoin, a planned incentivised network over IPFS aims to use its altcoin blockchain, with proof of retrievability as part of mining. The consequences of these choices are far reaching.

These properties plays a big role in the low level differences.

#### devp2p vs libp2p:

swarm heavily relies on the Ethereum p2p network, using ethereum's devp2p (protocol multiplexing, message 
interleaving by framing, encryption, authentication, handshake and protocol message API standard, 
peer connection management support, node discovery) and leverages its robustness and most notably inherits its 
(audited and widely-praised) security properties.

IPFS uses libp2p network layer, a similarly advanced generic p2p solution. It is an in-house development based on
the mainline bittorrent dht implementation that stood the test of time but improved by state-of-the-art optmisations.
For historical accuracy, it seems that devp2p is heavily inspired by libp2p (Devcon0 IPFS talk in nov 2014 Berlin and earlier exchange between Juan Benet (IPFS) and Gav Wood & Alex Leverington (ETH))

The Ethereum devp2p provides a semipermanent connection pool over TCP. As a result of the ethereum ecosystem, 
many nodes are commited long term.
These properties turn out to support relatively novel solutions in both incentivisation and storage/retrieval.

Swarm, just like IPFS, implements key-based routing based on xor logarithmic distance (applied to the shared address space of node-ids and content hashes), however swarm uses a hybrid flavour of forwarding kademlia: rather than iterative 
lookups and filtering performed by the originator of a request relying on a larger pool of peers, swarm recursively outsource  the successive steps of lookup and use only a smaller pool of active connections. Further aspects of this algorithm are overly technical for the scope of this note.

Swarm is content addressed chunk archive while IPFS is more akin to bittorrent with a content addressed DHT (distributed hash table). 
A DHT is a distributed index which decentralised storage solutions use to look up content addressed  data. 
While this data is usually (IPFS) metadata about downloading the content, in swarm its the content itself. 
Note that the DHT is just one available protocol in IPFS (IPFS's layered design is highly modular).
This strict interpretation of an immutable content addressed chunkstore is a major design feature of swarm, which together with devp2p allow swarm to do: 

* efficient pairwise accounting offchain (used for fair incentivisation of bandwidth as well as immediate settlement of 
  insured storage)
* smoother automatic scaling on popular content 
* quasi-anonymous browsing
* efficient collective auditing of integrity (on rarely accessed content) offchain

Juan commented here that the kademlia DHT is just an optional routing component in IPFS and that it can actually do all these things. (My homework to figure out whether and how).

I believe both IPFS and swarm will offer streaming of encrypted content with integrity protection (even on partial content). 

#### Incentives 

Filecoin is a sister project of IPFS, it adds an incentivisation layer to IPFS and relies on its own altchain. 
Proof of retrievability "mining" on the filecoin blockchain is a scheme  providing ongoing compensation to storers for preserving content. Random audits as part of the proof of work task are
responded to with proof of retrievability and the winning miners get compensated accordingly. 
Such a system has inherent limitations: IPFS can only implement positive incentives and relies on collective responsibilty.

Swarm exploits the full capabilities of smart contracts to handle registered nodes with deposit to stake. This allows for 
punative measures as deterrents. Swarm provides a scheme to track responsibilities making storers individually accountable for particular content. 

IPFS has no guarantee for storage, while swarm enforces content agnostic behaviour as well as offers content-specific 
levels of security flexibly adjustable by the users. Juan commented here that they have been adding something this to filecoin which will also have a smart-contract blockchain, but these are as yet unpublished ideas and plans.

Swarm will implement efficient automated collective auditing of rarely accessed content off chain with last resort 
litigation on the blockchain as part of content insurance (a crucial feature). 
Using a pairwise accounting protocol and delayed micropayments off-chain swarm offers
substantial savings on transaction costs while maintaining security. IPFS+filecoin's reliance on competitive proof 
of custody mining means excessive use of blockchain and inherently redundant use of resources for normal operation.

With pairwise accounting and delayed payments and collective audits both offchain, swarm relies a lot less heavily 
on the blockchain restricting its use only to registration and last-resort litigation.

#### Manifests

Finally the swarm 'manifest' concept (universal routing table/key-value index with integrity protection) allows for 
* modeling hierarchical file systems on the cloud
* serverless servers with routing table and a principled system of metadata (content type, encryption and insurance info etc)
* implementing arbitrary DHTs within swarm, so it can support "sidechains" or the db component of traditional webapps 
  (c. mysql in LAMP stack etc)

Juan comments here that IPFS can do all these things too. Admittedly both projects are bit handwavy here without working code or doc...

##  Integration and collaboration

**tl;dr** though the big picture aligns well, there are challenges in integrating the two projects. A few
proposals are outlined offering pros and cons of each. 

Juan of IPFS has long been a regular at Ethereum events, the two communities seem to be mutually appreciative. 
The similarity of the two endeavors both in technical detail and with respect to the generic objective led many to the question if the two efforts could and should be somehow unified or coordinated. 
This has led some to believe that swarm is 'just an incentive layer on top of arbitrary decentralised storage'.
This misconception may have been encouraged also by the fact that during the tough times last autumn, the foundation had to limit the scope of the project and Vitalik (quite wisely) decided that development effort should be shifted to
include incentivisation but exclude R&D for parts that are taken care of by IPFS. 
The motivation for these restrictions no longer exist.
The philosophical/marketing differences, organisational affiliations, business models and technical dependencies make 
it very unlikely the two projects become one any time soon. 

### Integration of IPFS into swarm 

After a lot of thinking and going through IPFS docs and code i felt like commiting to some level of integraion. 
What follows is a preliminary list of various - admittedly rather speculative - approches.

* implement the IPFS plugin into swarm as an implementation of the cloud store abstraction of its network protocol
  * Pros:
     * implementing this requires the least development effort 
     * As i see now, this approach would even make the swarm incetive system to drive IPFS nodes.
     * This approach is promising ways to test the correctness and benchmark the efficiency of the two systems 
  * Cons: 
     * unclear if the swarm chunk-based storage enforced on IPFS style routing and retrieval would have any 
  performance gain
     * unclear if an ethereum-based incentive on top of IPFS would impact the IPFS user base etc.
     * unclear if using IPFS libp2p (alongside devp2p) adds security risk and or 
       makes network traffic monitoring/load balancing harder
     * unclear if this (ab)use of an IPFS component lends itself to realistic way of coordinating the 
     parallel development of IPFS and Swarm (here i am thinking of planning software updates etc). Juan comments here that he thinks it would be just fine ;)
    
  This solution is very very likely to be pursued due to its optimal gain/effort ratio
     
* work out and implement a simpler Ethereum based incentive layer better suited for IPFS. 
  * Pros: 
     * integrity of the entire mechanics of IPFS is preserved
     * the parallel development of IPFS and Swarm by separate teams on different schedules makes it somewhat
       more realistic to maintain long term than the first proposal
  * Cons:
     * Such an incentive layer (for IPFS) does not yet exist and frankly I personally find it hard to see it done properly 
     any time soon, 
     * The couple of crude ideas i had for a workable incentive scheme for IPFS would entail 
       compromise on the desired spec for a permanent web incentive system
     
* some crazy ideas for the lulz:
  * implement filecoin as a swarm sidechain 
  * consider migrating ethereum to libp2p and use IPFS with all its glory 

* Integrating swarm into IPFS as a subprotocol
  * Pros:
    * it's kind of cool
  * Cons:
    * real benefits unclear
    
* Mounting IPFS over the transport layer (RLPx) of devp2p - This has been suggested to me very recently by Juan and have not fully captured/investigated the prerequisited or consequences of this
  * Pros:
    * IPFS could potentially be used in all its glory
    * the devp2p integrity would be preserved
  * Cons:
    * ?? 

* Cross-semination of ideas and snatching code: 
  With all the above stronger forms of collaboration failing, i still see possible synergetic effect 
  mutually benefitting the two projects 
  * go implemention allows quick adaptation of relevant ideas now and in the future without pushing for strings attached
  * both IPFS and Ethereum being quite sexy projects, they strengthen each other's PR and increase the chances of success
  * given the incredible traction and rate of innovation in the cryto 2.0 / web3 space, it is highly likely that some 
    of my proposals above will change and new opportunities present themselves. 
  
  All in all there is a solid basis to continue being friends and use each other's resources in whatever (morally sound) 
  way to promote the shared objective of a free, private, resource-efficient serverless webz.

 Next step April 27-28 Berlin IPFS-Swarm synergistic entanglement & interplanetary crosspolination 
 
All the opinions and mistakes are mine only and i welcome feedback.

# Resources

The original stackoverflow question:
http://ethereum.stackexchange.com/questions/2138/what-is-the-difference-between-swarm-and-ipfs

## IPFS/SWARM on reddit:
* http://ethereum.stackexchange.com/questions/2138/what-is-the-difference-between-swarm-and-ipfs
* https://www.reddit.com/r/ethereum/comments/4bms6v/what_is_the_difference_between_swarm_and_ipfs/
* https://www.reddit.com/r/ethereum/comments/3npsoz/ethereum_ipfs_and_filecoin/
* https://www.reddit.com/r/ethereum/comments/3hbqbv/ipfs_vs_swarm/
* https://www.reddit.com/r/ethereum/comments/2z7iyr/a_polite_discussion_about_symbiosis_between_ipfs/

## IPFS
 
* https://ipfs.io
* DEVCON1 talk by Juan Benet - https://www.youtube.com/watch?v=ewpIi1y_KDc
* https://github.com/ipfs/specs/
* https://github.com/ipfs/faq/issues/
* https://github.com/ipfs/specs/blob/master/libp2p/
* https://github.com/ipfs/specs/blob/master/libp2p/4-architecture.md


## Swarm

### ÐΞVcon talks on swarm

* [Viktor Trón, Daniel A. Nagy: Swarm - Ethereum ÐΞVcon-1 talk on youtube](https://www.youtube.com/watch?v=VOC45AgZG5Q)
* [Daniel A. Nagy: Keeping the Public Record Safe and Accessible - Ethereum ÐΞVcon-0 talk on youtube](https://www.youtube.com/watch?v=QzYZQ03ON2o&list=PLJqWcTqh_zKEjpSej3ddtDOKPRGl_7MhS)

### ETHERSPHERE orange papers 
* Viktor Trón, Aron Fischer, Daniel A Nagy and Zsolt Felföldi: swap, swear and swindle: incentive system for swarm. [pdf](http://52.70.20.40:32200/bzz:/ethersphere/orange-papers/1/swap-swear-and-swindle.pdf)|[html](http://52.70.20.40:32200/bzz:/ethersphere/orange-papers/1/swap-swear-and-swindle.html)|[bibtex entry]((http://52.70.20.40:32200/bzz:/ethersphere/orange-papers/1/swap-swear-and-swindle.bib)
* Viktor Trón, Aron Fischer, Daniel Varga: smash-proof: auditable storage for swarm secured by masked audit secret hash. [pdf](http://52.70.20.40:32200/bzz:/ethersphere/orange-papers/2/smash.pdf)|[html](http://52.70.20.40:32200/bzz:/ethersphere/orange-papers/2/smash.html)|[bibtex entry]((http://52.70.20.40:32200/bzz:/ethersphere/orange-papers/2/smash.bib)

### code and status

* [source](https://github.com/ethereum/go-ethereum/tree/swarm)
* [issues on github](https://github.com/ethereum/go-ethereum/labels/swarm)
* [development roadmap]()

## follow swarm 

* [@ethershere on twitter](https://twitter.com/ethersphere)
* [gitter swarm room](https://gitter.im/ethereum/swarm) 
* [swarm on swarm: bzz://swarm](http://52.70.20.40:32200/bzz:/swarm) public gateway (thanks to  @TerekJudi|@uwaterloo) once testnet is public
