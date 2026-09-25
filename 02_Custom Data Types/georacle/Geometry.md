# Sample the geometry of a region

## Query Name

- `Geometry`

## Query Description

This query returns a list of coordinates' points near a given location id.

## Query Parameters

The `Geometry` query has one required parameter, which is the location id plus an option limit count. To get information on the max number of location ids available, query the GeometryCount query type that returns the max count possible and then add in to the limit count of this query for max amount of list of coordinates.  ```https://api.georacle.io/geometry/count/{id}```

1. **locationId** (string): the unique location id that reperesents a street address.
2. **sampleCount** (int256): an optional parameter for the count of coordinates wanted.


## Response Type

The query response will consist of an int64 list, see example below for decoding:

- `abi_type`: '(int64[])'
- `packed`: false

*Descriptor:*

```json
    {
        "type": "Geometry",
        "inputs": [{
            "type": "string",
            "name": "locationId",

            "type": "int256",
            "name": "sampleCount"},
        ],
        "outputs": [
            {
              "name": "(latitude, longitude)",
              "type": "int64[]",
              "packed":false
            }
        ]
    }
```
## Examples

### Tellor Geometry

*Query Descriptor:*

```json
{"type":"Geometry","locationId":"_gAAAAAareXQ","sampleCount":8}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.encode("Geometry", abi.encode("_gAAAAAareXQ",8))
    ```

- <details><summary>Python</summary>

    ```python
    queryDataArgs = encode_abi(["string","int256"], ["_gAAAAAareXQ",8])
    queryData = encode_abi(["string", "bytes"], ["Geometry", queryDataArgs])
    ```

</details>
</details>

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000847656f6d65747279000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c5f67414141414161726558510000000000000000000000000000000000000000`

*queryID:*

- <details><summary>Python</summary>

    ```python
    queryId = '0x' + bytes(Web3.keccak(queryData)).hex()
    ```
- <details><summary>Solidity</summary>
  
    ```javascript
    keccak256(queryData)
    ```
</details>
</details>

`0x8f0d60fd0618317a192398fc99982d877efb4e2c6a55986ef3bf21647da24b58`


### Encoding/Decoding

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    function decodeSize(bytes memory data)
        internal
        pure
        returns (uint256 size)
        {
        int256 value;
        assembly {
            value := mload(add(data, 32))
            }
        size = uint256(((value & (mask_64_t << 0xc0)) >> 0xc0));
        }
    }
    function decodePoints(bytes memory data)
        internal
        pure
        returns (int64[] memory)
    {
        int256 value;
        uint256 index = 0;
        uint256 size = decodeSize(data);
        int64[] memory result = new int64[](size);
        for (uint256 i = 0; i < data.length; i += 32) {
            assembly {
                value := mload(add(data, add(i, 32)))
            }
            if (i > 0 && index < size)
                result[index++] = int64((value & (mask_64_t << 0xc0)) >> 0xc0);
            if (index < size)
                result[index++] = int64((value & (mask_64_t << 0x80)) >> 0x80);
            if (index < size)
                result[index++] = int64((value & (mask_64_t << 0x40)) >> 0x40);
            if (index < size) result[index++] = int64((value & mask_64_t));
        }
        return result;
    }
    ```
    Full code: https://github.com/georacleapi/georacle/blob/258bdedfcfb06276ec70f4b86df0a4e2b3b34608/src/v0.7/Georacle.sol#L304

- <details><summary>Python</summary>
  
    ```python
    def decode(response) -> List[float]:
        res = response
        if len(res) < 10:
            raise ValueError
        payload = bytes.fromhex(res[2:])

        pack_len = int.from_bytes(payload[0:8], byteorder="big")
        if len(payload) < ((pack_len + 1) << 3) + 8:
            raise ValueError(
                "Invalid geometry encoding. Check your query parameters."
            )
        pts = []
        for i in range(0, pack_len - 1, 2):
            lat = int.from_bytes(
                payload[(i + 1) << 3 : (i + 2) << 3], byteorder="big", signed=True
            )
            lon = int.from_bytes(
                payload[(i + 2) << 3 : (i + 3) << 3], byteorder="big", signed=True
            )
            pts.append((lat / Precision, lon / Precision))

        return pts
    d = bytes.fromhex(""[2:])
    coordinatesList = decode("0x000000000000001000000000025969affffffffffb62cd5a0000000002596c0ffffffffffb62cdaa0000000002596df0fffffffffb62cdea0000000002596e4afffffffffb62cdf50000000002596e7bfffffffffb62cdfb00000000025971befffffffffb62ce5f00000000025972c4fffffffffb62ce7c0000000002597322fffffffffb62ce86000000000000000000000000000000000000000000000000")
    ```
    Full code: https://github.com/georacleapi/pygeoracle/blob/b056ea0ad93a62a760d8d7bf4e0963203cda7a92/georacle/endpoints/geometry.py#L51
</details>
</details>

A value of [(39.414191, -77.410982),(39.414799, -77.410902),(39.41528, -77.410838),(39.41537, -77.410827),(39.415419, -77.410821),(39.416254, -77.410721),(39.416516, -77.410692),(39.41661, -77.410682)] would be submitted on-chain using the following bytes:

`0x000000000000001000000000025969affffffffffb62cd5a0000000002596c0ffffffffffb62cdaa0000000002596df0fffffffffb62cdea0000000002596e4afffffffffb62cdf50000000002596e7bfffffffffb62cdfb00000000025971befffffffffb62ce5f00000000025972c4fffffffffb62ce7c0000000002597322fffffffffb62ce86000000000000000000000000000000000000000000000000`


## Sources

Use georacle.io, https://api.georacle.io/geometry, source to get this data.  
```https://api.georacle.io/geometry/{id}&api_key={api_key}``` You have to include a header with this request. ```Header = {"user-agent": ""}```
