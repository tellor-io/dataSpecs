---
Title: PolygonBridge.md
Author: Brenda Loya
Created: 2021-09-13
---
# Request ID: 

keccack256(abi.encode([string,address,bytes4,bytes,uint],["PolygonBridge, _contract, _function,_data, block.number]))

In order to be a valid ID, the keccack256 of the input data in the addTip function must equal the requestID. 

# Data Specifications

This allows the Tellor Oracle to bring data back from the Polygon network.  The ID and the arguments for retrieving data include several pieces: the contract address, the function call (read only), function inputData, and the block number.



## Description

This is the feed for the ETH/USD price. It represents a valid ETH/USD price at the time the data is placed on-chain.  


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
