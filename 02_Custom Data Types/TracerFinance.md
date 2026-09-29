## Type Name

- `TracerFinance`


## Description

This query returns the result for a given poolId (a specific Tracer pool) on Tracer
Finance on Arbitrum

For complete information, see their docs: https://docs.tracer.finance/tracer/master

To get the pool information, use TheGraph(https://thegraph.com/hosted-service/subgraph/scaredibis/tracer-pools-v1-arbitrum-one?query=Example%20query) to query Tracer.finance, a subsequent
query can help you:

```
{
  leveragedPools(
    where:{
      id:"0x146808f54db24be2902ca9f595ad8f27f56b2e76"
    }
  ) {
      name
  }
}
```
response:

```json
{
  "data": {
    "leveragedPools": [
      {
        "name": "1-BTC/USD"
      }
    ]
  }
}
```
where:
- 1: the pool's leverage
- BTC/USD: reference asset

## Query Parameters

The `TracerFinance` query has one parameter, which specifies the requested data.

1. **poolId** 

- description: Tracer pool ID (e.g. 1)
- value type: `uint256`


## Response Type
The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false


## Query Data


```s
queryData = abi.encode("TracerFinance", abi.encode(1))
```
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000d54726163657246696e616e63650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000001`

## Query ID

```s
keccak256(queryData)
```

`0xd2987fe526cc6643eebb49e9d182f2f016d4af5ff4c656cef7190c4c9e4026a9`


## JSON Representation
```json
{
    "type": "TracerFinance",
    "abi": [
        {
            "type": "uint256",
            "name": "poolId",
        },
    ],
    "response": {
        "type": "ufixed256x18",
        "packed": false,
    }
}
```


## Example
A working example mapping of all the various inputs and parameters to a valid queryID. 
`keccak256(abi.encode("TracerFinance",abi.encode(1)))`

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.

## Dispute Considerations

- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.
- It is the reporters responsibility to ensure that the feed result is reasonable enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted