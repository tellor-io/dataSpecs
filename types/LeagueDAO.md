# LeagueDAO Feed

## Query Name

- `LeagueDAO`

## Query Description

This query returns the result for LeagueDAO

## Query Parameters

The `LeagueDAO` query has X parameter, which specifies the requested data.  

1. **xxxx** (uint256): ID of the prediction market

The `xxxx` should be X

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

*Query Descriptor:*

```json
    {
        "type": "LeagueDAO",
        "inputs": [{
            "type": "uint256",
            "name": "optionID"
        }],
        "outputs": [
            {
              "name": "LeagureDAOValue",
              "decimals": 18,
              "type": "uint256",
              "packed":false
            }
        ]
    }
```

```s
    abi.encode("LeagueDAO",1)
```

*queryID:*

    keccak256(abi.encode("LeagueDAO",abi.encode(1)))

    `0x42ab85a3fe992d1c1c916644f82a42c37a30419be206774cb48a7adc70954995`

### Encoding/Decoding Results



## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.
