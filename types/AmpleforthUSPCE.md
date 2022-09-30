## AmpleforthUSPCE

## Description

This query returns the current United States Personal Consumption Expenditures (USPCE), as specified by the Ampleforth protocol.


## Query Parameters

The `AmpleforthUSPCE` query has one parameter, which is always an empty `bytes` array.

1. **phantom**
   - description: Empty bytes, always used for query types with no arguments
   - value type: `bytes`

Parameters for this query type should never change, so there is only one possible query id for `AmpleforthUSPCE`.


## Response Type

The response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Query Data

The query data should be formed as follows:

```s
queryData = abi.encode("AmpleforthUSPCE", abi.encode(bytes("")))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000f416d706c65666f72746855535043450000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```

`0x612ec1d9cee860bb87deb6370ed0ae43345c9302c085c1dfc4c207cbec2970d7`


## JSON Representation
The JSON representation of a query type is needed to construct query objects in a variety of languages. It contains the essential components of a query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

The JSON representation of a `AmpleforthUSPCE` query is as follows:
```json
{
    "type": "AmpleforthUSPCE",
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

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  

## Suggested Data Sources

The sources and calculation for this query type is specified by the Ampleforth protocol. See the [Telliot](https://github.com/tellor-io/telliot-feeds) codebase for an example implementation.
