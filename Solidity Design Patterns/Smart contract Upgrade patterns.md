
- Change business logic but preserve contract state
- 5 patterns : 

# 1. Contract migration

- Deploying new version of contract. 
- 2 steps:
	- 1. Recover data to migrate
	- 2. Write data to the new contract
- High cost are associated with contract migration
- Recovering data can be done offchain with Google Big query API and ethereum-etl
	- Recovering is done by tracking all events related to contract form the initial block
- Writing data can be expensive as per the data structure

Eg: For ERC20 token migration of address and balances

```js 
function batchTransfer(address [] destination, uint256 [] values) duringInitialzation	onlyOwner external{
	require(destination.length == values.length);
	uint256 length = destination.length;
	for(unit256 i =0; i < length;i++){
		balances[destination[i]] = values[i];
		emit Transfer("0x00",destination[i],values[i]);
	}
}
```

- Gas limit of the block should be noted before the transfer is initiated (If gas block limit is exceeded then the transaction is not included in the block)
- *** Users should be use the the new contract
- *** Exchanges should be informed of the new contract

# 2 . Data Seperation

Contract is divided into business logic and storage logic

- Storage contract holds the contract state, is immutable and links to logic contract
- Logic contract contains logic that updates state in storage contract
- Logic contract can be changed by pointing to new logic contract is storage contract


![[My canvas]]