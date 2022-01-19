---
Title: ETH/USD feed
Author: Brenda Loya
Created: 2021-09-27
---
## Query Name

- `LegacyRequest`

## Query Description

This is the type for all legacy requests (created and running before 12/01/2021). Unlike all new ID's, the hash of the _queryData is not the _queryID for legacy ID's. 

## Query Parameters

The `LegacyRequest` query has one parameter, which specifies the requested data.  

1. **requestId** (uint256): ID of the Tellor request

The `requestId` should be a requestId from the Tellor system before the TellorX upgrade. 


## Granularity

1000000 (6 decimals)

## Example Value (w/ date)

9/27/21 - 3000000000


## Supported Data

Only certain ID's are valid for legacy requests.  The others will be considered null and any price feeds wanted other than this list should use the SpotPrice.md type

Supported:
    - 1
    - 2
    - 10
    - 41
    - 50
    - 59

# Dispute Considerations

Be sure to use multiple exchanges, as one exchange could potentially slip out of alignment with others.  Also, be sure to note w/ regard to the "current" specification, that Ethereum blocks can take time to mine and prices move quickly.  A valid answer may not correspond to reality when it gets accepted in a block. 

# Notes

-

