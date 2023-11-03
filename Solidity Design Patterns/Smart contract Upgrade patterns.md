
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
- Make old logic contract pausible :
``` sol
contract Pausible{
	
	event paused();
	event unpaused();
	
	bool paused = false;
	uint256 data;

	function setdata(uint256 value)public {
		require(!paused, "Contract is paused");
		data = value;
	}

	function pause()public{
		require(!paused, "Contract is already paused");
		paused = true;
	}


	function unpause() public {
		require(paused, "Contract is already unpaused");
		paused = false;
	}
}

```


![[Data seperation.canvas|Data seperation]]
![[Dataseperation.png]]

# 3. Proxy Contract

-  Opposite of data separation: 
	- User interact with proxy contract which holds the storage of the contract
	- Proxy calls logic contract to update the state in Proxy contract
- Proxy contract changes pointer to another logic contract during upgrade

![[Proxydelegate.canvas|Proxydelegate]]
Proxy upgrade approaches:

1. Inherited Storage
	- Both proxy and logic contract contains same memory layout through inheritance from a memory layout
	- Upgraded contract should hold same memory layout + additional state and functionality
	![[inheritedstorage.canvas|inheritedstorage]]
2. Eternal Storage
	- Similar to inherited storage but state stored in mapping
3. Unstructured storage
	- Storage variables in proxy contract is stored at a randomized slot so that there is no storage collision with the logic contract
	- Completely immune to data storage collision
	- Randomized storage is achieved by:
	```sol
	bytes32 private constant implementationPosition = 
	bytes32(uint256( keccak256('eip1967.proxy.implementation')) - 1 ));
	```
	![[beforeUnstructuredStorage.png]]
	![[AfterUnstructuredStorage.png]]

	Risk: 
		- Handling delegate call securely is hard task. (Chances of extreme vulnerability if delegate call is implemented with improper knowledge)
		- Changes of storage collision due to delegate call