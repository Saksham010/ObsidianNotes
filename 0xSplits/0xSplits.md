# Introduction
	- Onchain payment splitting protocol
	- Free (No protocol fee, only gas fee)
	- Multichain protocol
	- Composable (Uplook)
	- Batched operation improves gas efficiency
	- Incentivized - Probably like intents
# Flow of funds
	# Receiving
		- Funds are sent by specifying another contract/EOA address
		- Then, funds are received by the smart contract and stored         until distribution occurs.
	
	# Distribution
		- Funds in the contract's balance will be allocated to each         recepients on basis of percentage according to the rule of        the protocol.
		- Waterfall (Related Uplook)
	
	# Withdrawing
		- Smell intent action overhere

# Associated design patterns
	
	- Pull over push approach during multi ether transfer

# Core contracts:
	- Split (Percentage based)
	- Waterfall (Tranches based)