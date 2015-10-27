# Node Discovery Protocol

## **THIS PAGE IS OUTDATED - please see [RLPx spec](https://github.com/ethereum/devp2p/blob/master/rlpx.md) for the up to date version**

**Node**: an entity on the network  
**Node ID**: 512 bit public key of node

The Node Discovery protocol provides a way to find RLPx nodes to connected to. It uses a Kademlia-like protocol to maintain a distributed database of the IDs and endpoints of all listening nodes.

Each node keeps a node table as described in the Kademlia paper [[Maymounkov, Mazières 2002][kad-paper]]. The node table is configured with a bucket size of 16 (denoted `k` in Kademlia), and concurrency of 3 (denoted `α` in Kademlia). The idle bucket-refresh interval is 3600 seconds.

To maintain a well-formed network, RLPx nodes should try to connect to an unspecified number of close nodes. To increase resilience against Sybil attacks, nodes should also connect to randomly chosen, non-close nodes.

Each node runs the UDP-based RPC protocol defined below. The `FIND_DATA` and `STORE` requests from the Kademlia paper are not part of the protocol since the Node Discovery Protocol does not provide DHT functionality.

[kad-paper]: http://www.cs.rice.edu/Conferences/IPTPS02/109.pdf

## Joining the network

When a node joins the network, it fills its node table by a recursive `Find Node` operation with its own ID as the `Target`. The initial `Find Node` request is sent to one or more bootstrap nodes.

## RPC Protocol

RLPx nodes that want to accept incoming connections should listen on the same port number for UDP packets (Node Discovery Protocol) and TCP connections (RLPx protocol).

All requests time out after 300ms. Requests are not re-sent.

UDP packets:

Offset  |||
------: | ----------| -------------------------------------------------------------------------
0       | MDC       | Ensures integrity of packet, `SHA3(signature || type || data)`
32      | signature | Ensures authenticity of sender, `SIGN(sender-privkey, SHA3(type || data))`
97      | type      | Single byte in range [1, 4] that determines the structure of Packet Data
98      | data      | RLP encoded, see section Packet Data

The packets are signed and authenticated. The sender's Node ID is determined by recovering the public key from the signature.

    sender-pubkey = ECRECOVER(signature)

The integrity of the packet can be verified by computing the expected MDC of the packet as:

    MDC = SHA3(signature || type || data)

## Packet Data

All packets contain an `Expiration` date to guard against replay attacks. The date should be interpreted as a UNIX timestamp. The receiver should discard any packet with `Expiration` value in the past.

### Ping (type 0x01)

Ping packets can be sent and received at any time. The receiver should reply with a Pong packet. When the sender receives a Pong to her Ping, she must update the IP/Port of the receiver in her node table. If a node receives a ping, it will, after answering with a pong, send it's own ping to the first node. This ensures a node cannot trick a node through [IP address spoofing](https://en.wikipedia.org/wiki/IP_address_spoofing)

RLP encoding: **[** `IP`, `Port`, `Expiration` **]**

Element   ||
----------|------------------------------------------------------------

`IP`      | IP address (ASCII string) on which the node is listening
`Port`    | listening port of the node

### Pong (type 0x02)

Pong is the reply to a Ping packet.

RLP encoding: **[** `Reply Token`, `Expiration` **]**

Element       ||
--------------|-----------------------------------------------
`Reply Token` | content of the MDC element of the Ping packet

### Find Node (type 0x03)

Find Node packets are sent to locate nodes close to a given target ID.
The receiver should reply with a Neighbors packet containing the `k`
nodes closest to target that it knows about.

RLP encoding: **[** `Target`, `Expiration` **]**

Element  ||
---------|--------------------
`Target` | is the target ID

### Neighbors (type 0x04)

Neighbors is the reply to Find Node. It contains up to `k` nodes that
the sender knows which are closest to the requested `Target`.

RLP encoding: **[ [** `Node₁`, `Node₂`, ..., `Nodeₙ` **]**, `Expiration` **]**  
Each `Node` is a list of the form **[** `IP`, `Port`, `ID` **]**

Element   ||
----------|---------------------------------------------------------------
`IP`      | IP address (ASCII string) on which the node is listening
`Port`    | listening port of the node
`ID`      | The advertised node's public key

