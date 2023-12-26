# Spot Price BRC20 Feeds

## Query Name

- `SpotPriceBRC20`

## Query Description

This query returns the price of a non-crypto asset like a stock, derivatives product, or legacy market commodity.

## Query Parameters

The `SpotPriceBRC20` query has two parameters. The asset and currencty parameters specify the pricing pair.

1. **asset**
    - description: Asset ID (e.g. ORDI)
    - value type: `string`
2. **currency**
    - description: Selected currency (e.g. USD)
    - value type: `string`

If you would like to request a new custom price query, feel free to reach out to the Tellor team or make an issue/PR in this repository.

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Examples

### ORDI/USD Price

*Query Descriptor:*

```json
{"type":"SpotPriceBRC20","asset":"ordi","currency":"usd"}
```

*queryData:*

```s
abi.encode("SpotPriceBRC20", abi.encode("ordi", "usd"))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000e53706f745072696365425243323000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000046f7264690000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0xf1a84e9c12a7247678e9b10bc427e9b881907d62a7be05ff40d6cf2e3a85e3be`

     
### Encoding/Decoding

A value of 99.9 would be submitted on-chain using the following bytes:

`0x0000000000000000000000000000000000000000000000056a6418b505860000`

## Dispute Considerations

Reporters should seek guidance from the intended data consumer when selecting data sources and choosing the algorithm to combine them.

- Multiple sources should be used whenever possible.
- When retrieving data directly from and api (exchanges), feed users might expect that exchanges with lower volumes
have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a social consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted.
