# Diva Protocol Specific Feeds

## Query Name

- `DivaProtocolPolygon`

## Query Description

This query returns the result for a given poolID (a specific prediction market) on the Diva Protocol on Polygon

## Query Paramete

The `DivaProtocolPolygon` query has one parameter, which specifies the requested data.  

1. **poolID** (uint256): ID of the prediction market

The `poolID` should be a valid prediction market on the divaProtocol on the Polygon network, ready to be settled. 

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

*Query Descriptor:*

```json
    {
        "type": "DivaProtocolPolygon",
        "inputs": [{
            "type": "uint256",
            "name": "poolID"
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

```s
    abi.encode("DivaProtocolPolygon",1)
```

*queryID:*

    keccak256(abi.encode("DivaProtocolPolygon",abi.encode(1)))

    `0x4c22f2b746b4a712fe71df38c058d067baca27b3f97dbb132cfc7c2fce511379`

### Encoding/Decoding

A value of 99.9 would be submitted on-chain using the following bytes:

    0x0000000000000000000000000000000000000000000000056ba3d73af34eec04


## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- The prediction market nature of Diva Protocol means that the query could have a result that is a price, a temperature, a boolean, a sports score, or any different format for the data.  
- The result should be the correct one AT THE TIME of expiration. This means that if the prediction market bets on the price of BTC/USD on Feb 1st at midnight, the query should always return the price of BTC/USD at midnight on Feb 1st, even if it get called/requested again. 
- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted