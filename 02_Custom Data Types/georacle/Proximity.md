# Searching locations by proximity

## Query Name

- `Proximity`

## Query Description

This query returns a list of location ids that are within proximity of a given locations' coordinates (latitude, longitude), radius and some attributes. For instance, a list of restaurants within a certain distance of a location. The key attribute in this example would be amenity and the value attribute would restaurant. Also, you can limit the response length by including an optional parameter for the list count.  To retrieve information about the length of the list, it's helpful to use a proximity count query with this query type, e.g. https://api.georacle.io/location/proximity/count?lat={latitude}&lon={longitude}&radius={radius}&key={keyAttribute}&value={valueAttribute}.  This endpoint returns the max count available in the given proximity (lat,lon,radius)

## Query Parameters

The `Proximity` query has five required parameters plus an optional sixth.  

1. **latitude** (int256): e.g. 39415703
2. **longitude** (int256): e.g. -77410696
3. **radius** (int256): e.g. 500000000 in meters
4. **keyAttribute** (string): e.g. "amenity"
5. **valueAttribute** (string): e.g. "restaurant"
6. **limitCount** (int256): e.g. 4

*Descriptor:*

```json
    {
        "type": "Proximity",
        "inputs": [{
            "type": "int256",
            "name": "latitude",

            "type": "int256",
            "name": "longitude",

            "type": "int256",
            "name": "radius",

            "type": "string",
            "name": "keyAttribute",
            
            "type": "string",
            "name": "valueAttribute",
            
            "type": "int256",
            "name": "limitCount"},
        ],
        "outputs": [
            {
              "name": "idList",
              "type": "string[]",
              "packed":false
            }
        ]
    }
```

## Response Type

The query response will consist of a list of string values in the following format:

- `abi_type`: '(string[])'
- `packed`: false

## Examples

### Four restaurants near Tellor

*Query Descriptor:*

```json
{"type":"Proximity","latitude":39415703, "longitude":-77410696,"keyAttribute": "amenity", "valueAttribute": "restaurant", "limitCount": 4}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.encode("Proximity", abi.encode(39415703,-77410696,500000000,"amenity","restaurant",4))
    ```

- <details><summary>Python</summary>

    ```python
    queryDataArgs = encode_abi(["int256","int256","int256","string","string","int256"], [39415703,-77410696,500000000,"amenity","restaurant",4])
    queryData = encode_abi(["string", "bytes"], ["Proximity", queryDataArgs])
    ```

</details>
</details>

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000950726f78696d697479000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001400000000000000000000000000000000000000000000000000000000002596f97fffffffffffffffffffffffffffffffffffffffffffffffffffffffffb62ce78000000000000000000000000000000000000000000000000000000001dcd650000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000007616d656e69747900000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a72657374617572616e7400000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0x65f6f4e74805e9170fd0a682476abc869cc8419153c6a4318e84beaff5a56410`


### Encoding/Decoding

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.decode(data, (string[]))
    ```

- <details><summary>Python</summary>

    ```python
    d = bytes.fromhex("0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000000000000000000000000000000c5f77414141414341494e63390000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f77414141414341494e64470000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f77414141414341494e64610000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f77414141414464787a54610000000000000000000000000000000000000000"[2:])
    locationidList = decode_single('(string[])',d)
    ```

</details>
</details>

A value of (('_wAAAACAINc9', '_wAAAACAINdG', '_wAAAACAINda', '_wAAAADdxzTa'),),) would be submitted on-chain using the following bytes:

`0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000140000000000000000000000000000000000000000000000000000000000000000c5f77414141414341494e63390000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f77414141414341494e64470000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f77414141414341494e64610000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f77414141414464787a54610000000000000000000000000000000000000000`


## Sources

Use georacle.io, https://api.georacle.io/location/proximity source to get this data.https://api.georacle.io/location/proximity?lat={latitude}&lon={longitude}&radius=radius&key={keyAttribute}&value={valueAttribute}&limit={limitCount}&api_key={apiKey}. You have to include a header with this request. ```Header = {"user-agent": ""}```
