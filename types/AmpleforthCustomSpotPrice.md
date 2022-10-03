## AmpleforthCustomSpotPrice

## Description

This query returns the AMPL token volume weighted average price, as specified by the Ampleforth protocol.


## Query Parameters

The `AmpleforthCustomSpotPrice` query has one parameter, which is always an empty `bytes` array.

1. **phantom**
   - description: Empty bytes, always used for query types with no arguments
   - value type: `bytes`

Parameters for this query type should never change, so there is only one possible query id for `AmpleforthCustomSpotPrice`.


## Response Type

The response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Query Data

The query data should be formed as follows:

```s
queryData = abi.encode("AmpleforthCustomSpotPrice", abi.encode(bytes("")))
```

`0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000019416d706c65666f727468437573746f6d53706f74507269636500000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```

`0x0d12ad49193163bbbeff4e6db8294ced23ff8605359fd666799d4e25a3aa0e3a`


## JSON Representation
The JSON representation of a query type is needed to construct query objects in a variety of languages. It contains the essential components of a query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

The JSON representation of a `AmpleforthCustomSpotPrice` query is as follows:
```json
{
    "type": "AmpleforthCustomSpotPrice",
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
