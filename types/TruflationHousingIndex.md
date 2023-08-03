## TruflationHousingIndex

## Description

This query returns the CPI as reported by Trueflation.


## Query Parameters

The `TruflationHousingIndex` query has one parameter, which is always an empty `bytes` array.

1. **phantom**
   - description: Empty bytes, always used for query types with no arguments
   - value type: `bytes`

Parameters for this query type should never change, so there is only one possible query id for `TruflationHousingIndex`.


## Response Type

The response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Query Data

The query data should be formed as follows:

```s
queryData = abi.encode("TruflationHousingIndex", abi.encode(bytes("")))
```

`0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000016547275666c6174696f6e486f7573696e67496e64657800000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```

`0x6c87c0a9b50ff809e7a158a076411837589ec0d13bc6abddcfe23131975316c4`


## JSON Representation
The JSON representation of a query type is needed to construct query objects in a variety of languages. It contains the essential components of a query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

The JSON representation of a `TruflationHousingIndex` query is as follows:
```json
{
    "type": "TruflationHousingIndex",
    "abi": [
        {
            "type": "bytes",
            "name": "phantom",
        }
    ],
    "response": {
        "type": "ufixed256x18",
        "packed": false,
    }
}
```

## Dispute Considerations

Truflation Housing Index should be as shown at https://truflation.com/. 

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  

## Suggested Data Sources

https://truflation.com/ 
