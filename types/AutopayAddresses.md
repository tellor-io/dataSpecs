## AutopayAddresses

## Description

This query returns an array of current Tellor autopay addresses. It is used for retrieving user vote weights for disputes in the governance contract.


## Query Parameters

The `AutopayAddresses` query has one parameter, which is always an empty `bytes` array.

1. **phantom**
   - description: Empty bytes, always used for query types with no arguments
   - value type: `bytes`

Parameters for this query type should never change, so there is only one possible query id for `AutopayAddresses`.


## Response Type

The response will consist of an array of addresses:

- `abi_type`: address[]
- `packed`: false

## Query Data

The query data should be formed as follows:

```s
queryData = abi.encode("AutopayAddresses", abi.encode(bytes("")))
```

`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000104175746f70617941646472657373657300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```

`0x3ab34a189e35885414ac4e83c5a7faa9d8f03a4d530728ef516d203d91d6309c`


## JSON Representation
The JSON representation of a query type is needed to construct query objects in a variety of languages. It contains the essential components of a query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

The JSON representation of a `AutopayAddresses` query is as follows:
```json
{
    "type": "AutopayAddresses",
    "abi": [
        {
            "type": "bytes",
            "name": "phantom",
        }
    ],
    "response": {
        "type": "address[]",
        "packed": false,
    }
}
```


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.

## Suggested Data Sources

The means of establishing the Tellor autopay addresses shall be determined by the Tellor community through a combination of on-chain and off-chain signaling. Note that the correct list of autopay addresses may differ depending on the network on which it is being reported.
