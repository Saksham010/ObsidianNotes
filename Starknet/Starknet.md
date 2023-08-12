![[starknetFlow.png]]

Block hash : Pedersen hash of all block headers

<h2>Starknet Stack:</h2>
 - Layer 1 -> Data availability layer (Ethereum). 
 - Layer 2 -> Execution layer (Cairo VM). Rolled up transactions are executed
 - Layer 3 -> Application Layer (Indexer) -> Indexes useful information
 - Layer 5 -> Transport layer (JSON RPC, HTTPS) ->Communicates with L2 sequencer to include state changes in a block.
 ![[StarknetStack.png]]

<h2>Fractal Scaling</h2>
- Recursive starknet stack on top of original stack
![[FractalScaling.png]]