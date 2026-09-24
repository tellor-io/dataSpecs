## TellorOracleAddress

## Description

This query returns the latest Tellor oracle address. It is used for updating the time based rewards recipient on Ethereum mainnet.


## Query Parameters

The `TellorOracleAddress` query has one parameter, which is always an empty `bytes` array.

1. **phantom**
   - description: Empty bytes, always used for query types with no arguments
   - value type: `bytes`

Parameters for this query type should never change, so there is only one possible query id for `TellorOracleAddress`.


## Response Type

The response will consist of a single address:

- `abi_type`: address
- `packed`: false

## Query Data

The query data should be formed as follows:

```s
queryData = abi.encode("TellorOracleAddress", abi.encode(bytes("")))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000001354656c6c6f724f7261636c654164647265737300000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```

`0xcf0c5863be1cf3b948a9ff43290f931399765d051a60c3b23a4e098148b1f707`


## JSON Representation
The JSON representation of a query type is needed to construct query objects in a variety of languages. It contains the essential components of a query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

The JSON representation of a `TellorOracleAddress` query is as follows:
```json
{
    "type": "TellorOracleAddress",
    "abi": [
        {
            "type": "bytes",
            "name": "phantom",
        }
    ],
    "response": {
        "type": "address",
        "packed": false,
    }
}
```


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  

## Suggested Data Sources

The means of establishing the Tellor oracle address shall be determined by the Tellor community through a combination of on-chain and off-chain signaling.
