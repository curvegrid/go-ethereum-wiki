This one contract regulates the incentive structure of Swarm.

# Methods

## Sign up as a node

Pay a deposit in Ether and register public key. Comes with an accessor for checking that a node is signed up.

## Demand compensation for loss of chunk

Present a signed receipt by a signed up node. After a given deadline, the corresponding compensation is paid, unless a PoC (in the simplest case, the chunk itself) is presented. Comes with an accessor for checking that a given chunk has been lost (compensation has been paid).

## Present PoC to avoid paying compensation

No compensation is paid for lost chunks, if PoC is presented within the deadline. Comes with an accessor for checking that a given node is liable for compensation.