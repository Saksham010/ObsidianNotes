<h2>Preparing Transaction:</h2>
	 Sender queries the nonce, signs the transaction and sends to a node for broadcasting

<h2>Transaction Processing (Acceptance on L2): </h2>
	- The broadcasted transaction gets picked up by starknet network especially sequencers
	- Sequencers validate the transaction
	- If transactions are successfully executed then sequencer immediately updates its state
	- Status of Transaction changes to "Accepted on L2"
	- Transaction is included in a block and broadcasted
	- If sequencer is malicious then, the transaction could pass through this stage.
	- So, this stage does not provide complete finality but partial finality that relies on this L2 consensus of sequencers

<h2> Validation of proof (Acceptance on L1): </h2>
	- Prover re-executes the block of transaction produced by sequencers and produces proof for validity of the transactions.
	- Proofs are validated on ethereum by verifying smart contract.
	- If proofs are valid then transactions are included and the transaction status changes to "Accepted on L1"
	- This stage ensures finality of transaction with security from ethereum L1.


<h2> Phases of finality: </h2>
	<h5>Acceptance on L2 (Partial finality) -> Acceptance on L1 (Complete finality)</h5>
![[Transaction status.png]]