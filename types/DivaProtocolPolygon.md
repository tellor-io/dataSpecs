# Diva Protocol Specific Feeds

## Query Name

- `divaProtocolPolygon`

## Query Description

This query returns the result for a given poolId (a specific prediction market) on the Diva Protocol on Polygon

### Informaton of Query:

For complete information, see their docs: https://github.com/divaprotocol/oracles/tree/dataSpecs  

To get the pool information, run

```
    getPoolParameters(poolId)
```

The first result is the reference asset which should be returned for the value on expiry time (9th parameter)

e.g  -  referenceAsset: 'ETH/USDT',
e.g. - expiryDate: BigNumber { value: "1642021490" },

In this example, if poolId 1 refers to these data, you would return the ETH/USDT price at 1642021490

## Query Parameters

The `divaProtocolPolygon` query has one parameter, which specifies the requested data.  

1. **poolId** (uint256): ID of the prediction market. 

The `poolId` should be a valid prediction market on the divaProtocol on the Polygon network, ready to be settled. The parameter name `poolId` is the same name used within the Diva protocol contracts for a prediction market identifier.

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

*Query Descriptor:*

```json
    {
        "type": "divaProtocolPolygon",
        "inputs": [{
            "type": "uint256",
            "name": "poolId"
        }],
        "outputs": [
            {
              "name": "DivaProtocolValue",
              "decimals": 18,
              "type": "uint256",
              "packed":false
            }
        ]
    }
```

*queryData:*

```s
abi.encode("divaProtocolPolygon", abi.encode(1))
```

`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000136469766150726f746f636f6c506f6c79676f6e0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000001`

*queryID:*
```s
keccak256(abi.encode("divaProtocolPolygon",abi.encode(1)))
```

`0x769ef93b727c9435930f0b9aceae97f79afe68a1f368453835581395ca2e2474`

### Encoding/Decoding

A value of 99.9 would be submitted on-chain using the following bytes:

`0x0000000000000000000000000000000000000000000000056a6418b505860000`


## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- The prediction market nature of Diva Protocol means that the query could have a result that is a price, a temperature, a boolean, a sports score, or any different format for the data.  
- The result should be the correct one AT THE TIME of expiration. This means that if the prediction market bets on the price of BTC/USD on Feb 1st at midnight, the query should always return the price of BTC/USD at midnight on Feb 1st, even if it get called/requested again. 
- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted
