## InflationData

## Description

This query can be used for reporting various types of current inflation data to Tellor oracle networks.

## Query Parameters

The `InflationData` query has four parameters. The identifier parameter provides context for the data. The asset and currencty parameters specify the pricing pair.

1. **location**
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

## Query Data Examples

**The query data for the Bureau of Labor Statistics consumer price index (unadjusted) should be formed as follows:**

*Query Descriptor:*

```json
{"type":"InflationData","location":"USA","agency":"BLS","category":"","descriptor":"CPI"}
```

*queryData:*

```s
abi.encode("InflationData", abi.encode("USA","BLS","","CPI"))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000d496e666c6174696f6e44617461000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000160000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000000355534100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003424c530000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000034350490000000000000000000000000000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```
`0x389577823bc36cc44f87d115fcf1d681b9a12be480d449e85013a8cd788fc58a`

**The query data for the Beauro of Labor Statistics Shelter Category Unadjusted Index should be formed as follows:**

*Query Descriptor:*

```json
{"type":"InflationData","location":"USA","agency":"BLS","category":"shelter","descriptor":"unadjustedIndex"}
```

*queryData:*

```s
abi.encode("InflationData", abi.encode("USA","BLS","shelter","unadjustedIndex"))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000d496e666c6174696f6e44617461000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000180000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000000000000000000000000000000355534100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003424c53000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000077368656c74657200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000f756e61646a7573746564496e6465780000000000000000000000000000000000`

## Query ID

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```
`0x85c2b4b9a77f4985bab312d6eef5f08945070e1c84dc456a0eee4578bf6f64d8`


## JSON Representation
The JSON representation of a query type is needed to construct query objects in a variety of languages. It contains the essential components of a query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

The JSON representation of an `InflationData` query is as follows:
```json
{
  "type": "InflationData",
  "inputs": [
    {
      "type": "string",
      "name": "location",
      
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
