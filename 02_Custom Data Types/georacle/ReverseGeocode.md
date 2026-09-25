# Get coordinates for a street address

## Query Name

- `ReverseGeocode`

## Query Description

In contrast with the Geocode query type, this query takes in coordinates and returns the street address and location id.

## Query Parameters

The `ReverseGeocode` query has two required parameters, the latitude and longitude  coordinates.  

1. **latitude** (int256): Latitude coordinate (e.g. 39415703)
2. **longitude** (int256): Longitude coordinate (e.g. -77410696)

*Descriptor:*

```json
    {
        "type": "ReverseGeocode",
        "inputs": [{
            "type": "int256",
            "name": "latitude"}

            {"type": "int256",
            "name": "longitude"}
        ],
        "outputs": [
            {"name": "locationId",
              "type": "string",
              "packed":false},
              
            {"name": "locationAddress",
              "type": "string",
              "packed":false}
        ]
    }
```

## Response Type

The query response will consist of a string and two signed 256-bit values in the following format:

- `abi_type`: '(string,string)'
- `packed`: false

## Examples

### ReverseGeocode

*Query Descriptor:*

```json
{"type":"ReverseGeocode","latitude":39415703, "longitude":-77410696}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.encode("Geocode", abi.encode(39415703,-77410696))
    ```

- <details><summary>Python</summary>

    ```python
    queryDataArgs = encode_abi(["int256","int256], [39415703,-77410696])
    queryData = encode_abi(["string", "bytes"], ["ReverseGeocode", queryDataArgs])
    ```

</details>
</details>

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000e5265766572736547656f636f646500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000002596f97fffffffffffffffffffffffffffffffffffffffffffffffffffffffffb62ce78`

*queryID:*

```s
keccak256(queryData)
```

`0x40407e0f0ddd65fb0323d1695275b064b0720f743e6efaa5195b88ea716cbb60`


### Encoding/Decoding

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.decode(data, (string,string))
    ```

- <details><summary>Python</summary>

    ```python
    d = bytes.fromhex("0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000c5f6741414141414b6d74724400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000060537461726275636b732c203130342c204e6f727468204d61726b6574205374726565742c2046726564657269636b2c2046726564657269636b20436f756e74792c204d6172796c616e642c2032313730312c20556e6974656420537461746573"[2:])
    locationAddress = decode_single('(string,string)',d)
    ```

</details>
</details>

A value of ('_gAAAAAKmtrD',
 'Starbucks, 104, North Market Street, Frederick, Frederick County, Maryland, 21701, United States') would be submitted on-chain using the following bytes:

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000c5f6741414141414b6d74724400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000060537461726275636b732c203130342c204e6f727468204d61726b6574205374726565742c2046726564657269636b2c2046726564657269636b20436f756e74792c204d6172796c616e642c2032313730312c20556e6974656420537461746573`

## Sources

Use georacle.io, https://api.georacle.io/geocode/reverse, source to get this data.
https://api.georacle.io/geocode/reverse?lat=39415703&lon=-77410696&api_key={api_key}. You have to include a header with this request. ```Header = {"user-agent": ""}```
