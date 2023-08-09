## InflationData

## Description

This query can be used for reporting various types of current inflation data to Tellor oracle networks.

## Query Parameters

The `InflationData` query has four parameters. The identifier parameter provides context for the data. The asset and currencty parameters specify the pricing pair.

1. **region**
    - description: The area of the world where the data was collected (e.g. USA)
    - value type: `string`
2. **agency**
    - description: The acronym for or full name of the agency that collected the data (e.g. BLS)
    - value type: `string`
3. **category**
    - description: The category should describe which basket of goods is used to calculate inflation of (e.g. housing)
    - value type: `string`
4. **descriptor**
    - description: The descriptor should include additional information needed to differentiate the data. (e.g. index)
    - value type: `string`

To request new InflationData query, please reach out to the Tellor team or make an issue/PR in this repository.

## Response Type

The response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Query Data

The query data should be formed as follows:

*Query Descriptor:*

```json
{"type":"InflationData","region":"USA","agency":"BLS","category":"housing","descriptor":"index"}
```

*queryData:*

```s
abi.encode("InflationData", abi.encode("USA","BLS","housing","index"))
```

`00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000d496e666c6174696f6e44617461000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000180000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000000000000000000000000000000355534100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003424c5300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000007686f7573696e67000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000005696e646578000000000000000000000000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```

`0x675bc9f0a8e2819955d38f450f10f89a3c86e38b962bf03e57d39b25ea4550c3`


## JSON Representation
The JSON representation of a query type is needed to construct query objects in a variety of languages. It contains the essential components of a query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

The JSON representation of an `InflationData` query is as follows:
```json
{
  "type": "InflationData",
  "inputs": [
    {
      "type": "string",
      "name": "region",
      
      "type": "string",
      "name": "agency",

      "type": "string",
      "name": "category",

      "type": "string",
      "name": "descriptor",
    }
  ],
  "outputs": [
    {
      "type": "(string,string,string,string)",
      "packed": false
    }
  ]
}
```
### Encoding/Decoding

A value of 99.9 would be submitted on-chain using the following bytes:

`0x0000000000000000000000000000000000000000000000056a6418b505860000`

## Dispute Considerations

The InflationData query can be used for a wide range of data types. Dispute considerations should be discussed in the relavent reporter / user communities for each specific use case. 

## Suggested Data Sources

Data sources should be discussed in the relavent reporter / user communities for each specific use case.
