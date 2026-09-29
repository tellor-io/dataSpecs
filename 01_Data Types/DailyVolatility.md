
## Type Name

`DailyVolatility`


## Description

This is a query type for reporting a daily volatility index for an asset, `DailyVolatility`.


## Query Parameters

The parameters of the ``DailyVolatility`` query type will be the `asset`, `currency`, `days`.

```
1. asset
    - description: Asset ID (ex: ETH)
    - value type: `string`
2. currency
    - description: Selected currency (ex: USD)
    - value type: `string`
3. days
    - description: number of days (ex: Day)
    - value type: `string`
```


## Response Type

`DailyVolatility`'s response type is an unpacked 256 bit value with 18 decimals of precision.  The response will be the volatility index.
```
- abi_type: ufixed256x18 (18 decimals of precision)
- packed: false
```


## Query Data

```s
queryData = abi.encode("`DailyVolatility`", abi.encode("eth", "usd", 30))
```

## Query ID

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:

```s
keccak256(queryData)
```


## JSON Representation
The JSON representation of a `DailyVolatility` query:
```json
{
    "type": "`DailyVolatility`",
    "abi": [
        {
            "type": "string",
            "name": "asset",
        },
        {
            "type": "string",
            "name": "currency",
        },
        {
            "type": "uint256",
            "name": "days",
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

The queryData for a 30-day ETH/USD Volatility:

```s
queryData = abi.encode("DailyVolatility", abi.encode("eth", "usd", 30))
queryId = keccack256(queryData)
```

queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000f4461696c79566f6c6174696c697479000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000001e0000000000000000000000000000000000000000000000000000000000000003657468000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000`

queryId:
`0xd1ca89762fe6dfa00de99d8be3e7955bf1268b6f311a542bf37706f835ab23bf`

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 
