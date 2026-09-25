# Get coordinates to a street address

## Query Name

- `Geocode`

## Query Description

This query returns the locationID (a unique location identifier that's used for reference), longitude and latitude coordinates of a given location address. Locations are referenced by their unique identifier.

## Query Parameters

The `Geocode` query has one required parameter, which is the street address.  

1. **streetAddress** (string): Street Address (e.g. 118 N Market St Frederick MD 21701 USA)

*Descriptor:*

```json
    {
        "type": "Proximity",
        "inputs": [
            {"type": "string",
             "name": "streetAddress"}
        ],
        "outputs": [
            {
              "name": "locationId",
              "type": "string",
              "packed":false
            },
            {
              "name": "latitude",
              "type": "fixed256x6",
              "packed":false
            },
            {
              "name": "longitude",
              "type": "fixed256x6",
              "packed":false
            }
        ]
    }
```

## Response Type

The query response will consist of a string and two signed 256-bit values in the following format:

- `abi_type`: '(string, fixed256x6, fixed256x6)'
- `packed`: false

## Examples

### Tellor Geocode

*Query Descriptor:*

```json
{"type":"Geocode","streetAddress":"118 N Market St Frederick MD 21701 USA"}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.encode("Geocode", abi.encode("118 N Market St Frederick MD 21701 USA"))
    ```

- <details><summary>Python</summary>

    ```python
    queryDataArgs = encode_abi(["string"], ["118 N Market St Frederick MD 21701 USA"])
    queryData = encode_abi(["string", "bytes"], ["Geocode", queryDataArgs])
    ```

</details>
</details>

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000747656f636f646500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000026313138204e204d61726b65742053742046726564657269636b204d44203231373031205553410000000000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0xf8eee22d20faf0a680dfcd18cd0d81ce861909c02ee0c016ed914c01e75f309e`


### Encoding/Decoding

A value of ('_gAAAAAareXQ', Decimal('39.415703'), Decimal('-77.410696')) would be submitted on-chain using the following bytes:

`0x00000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000002596f97fffffffffffffffffffffffffffffffffffffffffffffffffffffffffb62ce78000000000000000000000000000000000000000000000000000000000000000c5f67414141414161726558510000000000000000000000000000000000000000`

### Geocode - Google headquarters

*Query Descriptor:*

```json
{"type":"Geocode","streetAddress":"1600 Amphitheatre Parkway Mountain View CA 94043 USA"}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.encode("Geocode", abi.encode("118 N Market St Frederick MD 21701 USA"))
    ```

- <details><summary>Python</summary>

    ```python
    queryDataArgs = encode_abi(["string"], ["1600 Amphitheatre Parkway Mountain View CA 94043 USA"])
    queryData = encode_abi(["string", "bytes"], ["Geocode", queryDataArgs])
    ```

</details>
</details>

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000747656f636f6465000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000343136303020416d70686974686561747265205061726b776179204d6f756e7461696e205669657720434120393430343320555341000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0xbd79f8ccda82aef0dd7ca0a682b673d8cd8c1e8ff8ff6df8113a54bca863903c`

### Encoding/Decoding

A value of ('_gAAAAABaiWb', 37422485, -122085584) would be submitted on-chain using the following bytes:
(coordinates are 6 decimals)

`0x000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000023b0595fffffffffffffffffffffffffffffffffffffffffffffffffffffffff8b91f30000000000000000000000000000000000000000000000000000000000000000c5f67414141414142616957620000000000000000000000000000000000000000`

## Considerations

Reporters need to use https://api.georacle.io/geocode source to submit this data.
https://api.georacle.io/geocode?address={streetAddress}&api_key={api_key}
