
## Type Name

`TWAP`


## Description

This is a proposal for a new query type for reporting a TWAP spot price to the Tellor network. The query type will be called `TWAP`.


## Query Parameters

The parameters of the `TWAP` query type will be the `asset`, `currency`, and `timespan`.

```
1. asset
    - description: Asset ID (ex: ETH)
    - value type: `string`
2. currency
    - description: selected currency (ex: USD)
    - value type: `string`
3. timespan
    - description: timespan of TWAP in seconds (ex: 86400 for one day)
    - value type: 'uint256`
```


## Response Type

`TWAP`'s response type is an unpacked 256 bit value with 18 decimals of precision. It's response type is measured in units of the base currency:
```
- abi_type: ufixed256x18 (18 decimals of precision)
- packed: false
```


## Query Data

```s
queryData = abi.encode("TWAP", abi.encode("eth", "usd", 86400))
```

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:

```s
keccak256(queryData)
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## JSON Representation
The JSON representation of a `TWAP` query:
```json
{
    "type": "TWAP",
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
            "name": "timespan",
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

The queryData for a 7-day ETH/USD TWAP:

```s
queryData = abi.encode("TWAP", abi.encode("eth", "usd", 604800))
queryId = keccack256(queryData)
```

queryData: `0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000004545741500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000093a800000000000000000000000000000000000000000000000000000000000000003657468000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000`

queryId:
`0x854365fb961fe425304355b8caf75acd71a565544d647af25aa842da13615ac5`

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

- Multiple sources should be used whenever possible.
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted
