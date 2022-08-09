# DIVA Protocol Specific Feeds

## Query Name

- `DIVAProtocolPolygon`

## Query Description

This query returns the required information to settle a pool from a DIVA Protocol deployment on Polygon.

### Information of Query:

For complete information, see their docs: https://github.com/divaprotocol/oracles/tree/main

## Query Parameters

The `DIVAProtocolPolygon` query has two parameters, `poolId` and `divaDiamond`.

1. **poolId**
    - description: Unique identifier of the prediction market (e.g. `1234`)
    - value type: `uint256`
2. **divaDiamond**
    - description: Contract address of DIVA Protocol (since there might be multiple deployments on a single network, e.g. `0xebBAA31B1Ebd727A1a42e71dC15E304aD8905211`)
    - value type: `address`

## Response Type

The query response will consist of a two floats represented as 256-bit integer values with 18 decimals of precision. These floats are the price of the reference asset and the price of the collateral token in USD, in that order.

To retrieve the reference asset and collateral token, see DIVA's documentation on pool parameters [here](https://github.com/divaprotocol/oracles/tree/main#diva-smart-contract)

- `abi_type`: `(ufixed256x18,ufixed256x18)`
- `packed`: false

An example of encoding and decoding this response type using Python:

```python
data = [2819.35, 0.9996]
submit_value = encode_abi(["uint256", "uint256"], [int(v * 1e18) for v in data])
print(submit_value)
```

_JSON Representation:_

```json
{
  "type": "DIVAProtocolPolygon",
  "abi": [
    {
      "type": "uint256",
      "name": "poolId"
    },
    {
      "type": "address",
      "name": "divaDiamond",
    }
  ],
  "response": {
    "type": "(ufixed256x18,ufixed256x18)",
    "packed": false
  }
}
```

_queryData:_

```s
abi.encode("DIVAProtocolPolygon", abi.encode(1234, 0xebBAA31B1Ebd727A1a42e71dC15E304aD8905211))
```

`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000134449564150726f746f636f6c506f6c79676f6e00000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000004d2000000000000000000000000ebbaa31b1ebd727a1a42e71dc15e304ad8905211`

_queryID:_

```s
keccak256(abi.encode("DIVAProtocolPolygon",abi.encode(1234, 0xebBAA31B1Ebd727A1a42e71dC15E304aD8905211)))
```

`0x9e2cc609c7cb305374b7390fba2769c71d3c806aea66937639892ee8c23371a3`

### Encoding/Decoding

For example, if the reference asset is ETH/USD with a price of $2,819.35, and the collateral token is DAI/USD at a price of $0.9996, then the hexadecimal of the bytes-encoded expected response for this query-- ready to be submitted by a reporter --is:

`0x000000000000000000000000000000000000000000000098d6574f71899000000000000000000000000000000000000000000000000000000ddf4ae7657b0000`

## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- The prediction market nature of DIVA Protocol means that the query could have a result that is a price, a temperature, a boolean, a sports score, or any different format for the data.
- The result should be the correct one AT THE TIME of expiration. This means that if the prediction market bets on the price of BTC/USD on Feb 1st at midnight, the query should always return the price of BTC/USD at midnight on Feb 1st, even if it get called/requested again.
- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.
- It is the reporters responsibility to ensure that the feed result is _reasonable_ enough for a community consensus, otherwise it may be subject to dispute.
- If a _reasonable_ value cannot be determined, a value should not be submitted
