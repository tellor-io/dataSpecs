# Get location ids for a specific regional box

## Query Name

- `BoundingBox`

## Query Description

Use this query to confine results of a search to a very specific regional space.  This query returns a list of location ids similar to the area query but has more parameters to constrain and focus the search area.  To get a max count before setting the limit in the request query ```https://api.georacle.io/location/bbox/count?south={coordiante}&west={coordiante}&north={coordiante}&east={coordiante}&key={key-attribute}&value={value-attribute}&limit=3&api_key={api_key}```

## Query Parameters

The `BoundingBox` query has six required parameters and an optional seventh.

1. **south** (int256): e.g. 40771900
2. **west** (int256): e.g. 73974600
3. **north** (int256): e.g. 40797500
4. **east** (int256): e.g. -73946900
5. **keyAttribute** (string): e.g. public_transport
6. **valueAttribute** (string): e.g. station
7. **limitCount** (int256): e.g. 3

*Descriptor:*

```json
    {
    "type": "BoundingBox",
    "inputs": [
        {"type": "int256",
         "name": "south",
        },
        {"type": "int256",
         "name": "west",
        },
        {"type": "int256",
         "name": "north",
        },
        {"type": "int256",
         "name": "east",
        },
        {"type": "string",
         "name": "keyAttribute",
        },
        {"type": "string",
         "name": "valueAttribute",
        },
        {"type": "int256",
         "name": "limitCount",
        },
    ],
    "output": [
        {"name": "idList",
         "type": "string[]",
         "packed":false}
    ]
}
```

## Response Type

The query response will consist of a string and two signed 256-bit values in the following format:

- `abi_type`: '(string[])'
- `packed`: false

## Examples

### Public Transportation in New York, Manahattan area

*Query Descriptor:*

```json
{"type":"BoundingBox","south": 40771900, "west": 73974600, "north": 40797500, "west": -73946900, "keyAttribute": "public_transport","valueAttribute": "station", "limitCount": 5}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.encode("BoundingBox", abi.encode(40771900, 73974600, 40797500, -73946900, 3))
    ```

- <details><summary>Python</summary>

    ```python
    queryDataArgs = encode_abi(["int256","int256","int256","int256", "string", "string", "int256"], [40771900,73974600,40797500,-73946900,"public_transport","station",3])
    queryData = encode_abi(["string", "bytes"], ["BoundingBox", queryDataArgs])
    ```

</details>
</details>

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b426f756e64696e67426f78000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000016000000000000000000000000000000000000000000000000000000000026e213c000000000000000000000000000000000000000000000000000000000468c34800000000000000000000000000000000000000000000000000000000026e853cfffffffffffffffffffffffffffffffffffffffffffffffffffffffffb97a8ec00000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000000300000000000000000000000000000000000000000000000000000000000000107075626c69635f7472616e73706f727400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000773746174696f6e00000000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0x302df8b688d350be9b43a44cb3470b122d01bbfe6cb9d62e7c00cf7ffdf67803`


### Encoding/Decoding

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.decode(data, (string[]))
    ```

- <details><summary>Python</summary>

    ```python
    d = bytes.fromhex("0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000000c5f77414141414150672d61480000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f7741414141416269614c550000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f7741414141424c4c4348780000000000000000000000000000000000000000"[2:])
    locationidList = decode_single('(string[])',d)
    ```

</details>
</details>

A value of (('_wAAAAAPg-aH', '_wAAAAAbiaLU', '_wAAAABLLCHx'),) would be submitted on-chain using the following bytes:

`0x00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000000c5f77414141414150672d61480000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f7741414141416269614c550000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c5f7741414141424c4c4348780000000000000000000000000000000000000000`


## Sources

Use georacle.io, https://api.georacle.io/location/bbox, source to get this data.
https://api.georacle.io/location/bbox?south={coordiante}&west={coordiante}&north={coordiante}&east={coordiante}&key={key-attribute}&value={value-attribute}&limit=3&api_key={api_key}. You have to include a header with this request. ```Header = {"user-agent": ""}```
