This one contract regulates the incentive structure of Swarm.

# Methods

## Sign up as a node

Pay a deposit in Ether and register public key. Comes with an accessor for checking that a node is signed up.

## Demand penalty for loss of chunk

Present a signed receipt by a signed up node and a deposit covering the upload of a chunk. After a given deadline, the signer node's deposit is taken and the presenting node's deposit refunded, unless the chunk is presented. Comes with an accessor for checking that a given chunk has been reported lost, so that holders of receipts by other swarm nodes can punish them as well for losing the chunk, which, in turn, incentivizes whoever holds the chunk to present it.

## Present chunk to avoid penalty

No penalty is paid for lost chunks, if chunk is presented within the deadline. The cost of uploading the chunk is compensated exactly from the demand's deposit, with the remainder refunded. Comes with an accessor for checking that a given node is liable for penalty, so the node is notified to present the chunk in a timely fashion.

# Price considerations

For the price of accepting a chunk for storing, see [Incentives](https://github.com/ethersphere/swarm/blob/master/doc/incentives.md)

The deposit for compensating the swarm node for uploading the chunk into the block chain should be substantially higher (e.g. a small integer multiple) of the corresponding upload measured with the gas price used to upload the demand to prevent DoS attacks.

