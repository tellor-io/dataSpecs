# Morphware Feed

## Query Name

- `Morphware`

## Query Description

This query returns the result for Morphware

## Query Parameters

The `Morphware` query has X parameter, which specifies the requested data.  

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

*Query Descriptor:*

```json
    {
        "type": "Morphware",
        "inputs": [{
            "type": "uint256",
            "name": "optionID"
        }],
        "outputs": [
            {
              "name": "Morphware",
              "decimals": 18,
              "type": "uint256",
              "packed":false
            }
        ]
    }
```

```s
    abi.encode("Morphware",1)
```

*queryID:*

    keccak256(abi.encode("Morphware",1))

    `0x4c6bfa4040303750318007ec849efdc93255109ff3c4e656171a6dfe1b35xxxx`

### Encoding/Decoding Results



## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.
