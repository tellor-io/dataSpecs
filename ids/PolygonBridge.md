---
Title: PolygonBridge 
Author: Brenda Loya
Created: 2021-09-13
---
# Data Spec ID: 5

# Request ID: 

keccack256(abi.encode([string,address,bytes4,bytes,uint],["PolygonBridge", _contract, _function,_data, block.number]))

In order to be a valid ID, the keccack256 of the input data in the addTip function must equal the requestID. 

# Data Specifications

This allows the Tellor Oracle to bring data back from the Polygon network.  The ID and the arguments for retrieving data include several pieces: the contract address, the function call (read only), function inputData, and the block number.


## Description

This is the feed to read data from Polygon.  The flexible structure of the Tellor oracle utilizes the EVM specifiations of Polygon to make arbitrary contract calls(reads) from the network and can then return them to the Ethereum network. 


## Granularity

1 - (raw bytes data, so no need to change it)

## Example Value (w/ date)

requestID   = keccack256("PolygonBridge",mesosphereContract,bytes4(keccack(balanceOf(address)),randomAddress,blockNumber))
            = 

Return Value = 
    (decoded)= 


## Dispute Considerations

Like all blockchains, forks happen. Be sure not to provide instaneous data from the front of the chain as finality may not be instantaneous. 

Requests can be future blocks, but the data can be zero if that block is not yet mined. It is up to the community to decide if the request was fufilled before the block was mined.  

# Notes

For those building using this data, note that this returns state from the polygon network only at 1 block.  For bridging purposes, be sure to know what the underlying rules for changing the state of your called data may be before relying on it to remain static. 

Note that Polygon also has a data bridge available through thier miners.  There may be cost or trust reasons to not want to use that bridge, but note that we are not trying to replace it.  Additionally, this data specification can work as an example for other bridging other chains and making arbitrary non-ETH calls and converting them to requestID's. 
