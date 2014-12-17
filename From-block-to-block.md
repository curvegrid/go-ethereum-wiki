This is a brief description on how block processing works (e.g., block 20 => block 21 => etc)

Block processing starts in the `ChainManager` (https://github.com/ethereum/go-ethereum/blob/docbranch/core/chain_manager.go)

* Block(s) are inserted using `InsertChain(...)` (https://github.com/ethereum/go-ethereum/blob/docbranch/core/chain_manager.go#L269)
* Each block is then processed by the `Processor` which in our case is always the `BlockManager` https://github.com/ethereum/go-ethereum/blob/docbranch/core/chain_manager.go#L271
    * Block validation and processing all happens with the `BlockManager` https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go
    * Checked if the block already's in storage (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L171)
    * Check if we have the parent (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L175)
    * Validate the easy, less intensive things first (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L195)
        * Calculate the total difficulty and compare it to the parent's TD (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L276)
        * Verify the proof of work (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L293)
    * Transition the state (e.g., apply transactions) (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L199)
    * Give coin base block gas limit which transactions use gas pool (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L88)
    * Create a new state transition object which will help us with the transition (https://github.com/ethereum/go-ethereum/blob/docbranch/core/block_manager.go#L117)
        * All transactional state changes happen within the `StateTransition` object and start at the `TransitionState` method (https://github.com/ethereum/go-ethereum/blob/docbranch/core/state_transition.go#L135)
        * Check the tx nonce: `tx.nonce == account.nonce` (https://github.com/ethereum/go-ethereum/blob/docbranch/core/state_transition.go#L123)
        * Check if we can buy the gas of the coin base (https://github.com/ethereum/go-ethereum/blob/docbranch/core/state_transition.go#L139)
        * Use TxGas (500) (https://github.com/ethereum/go-ethereum/blob/docbranch/core/state_transition.go#L154)
        * Create a contract if address = 0x0 * 32 (https://github.com/ethereum/go-ethereum/blob/docbranch/core/state_transition.go#L174)
        * OR run the code if the recipient has any code to execute (https://github.com/ethereum/go-ethereum/blob/docbranch/core/state_transition.go#L180)
        