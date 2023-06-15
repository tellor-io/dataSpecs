# Spot Price Feeds

## Query Name

- `CustomPrice`

## Query Description

This query returns the price of a non-crypto asset like a stock, derivatives product, or legacy market commodity.

## Query Parameters

The `CustomPrice` query has three parameters. The identifier parameter provides context for the data. The asset and currencty parameters specify the pricing pair.

1. **identifier**
    - description: Context ID (e.g. StockPrice)
    - value type: `string`
2. **asset**
    - description: Asset ID (e.g. NVDA)
    - value type: `string`
2. **currency**
    - description: Selected currency (e.g. USD)
    - value type: `string`

To request new custom price query, please reach out to the Tellor team or make an issue/PR in this repository.

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Examples

### STOCKPRICE/NVDA/USD Spot Price

*Query Descriptor:*

```json
{"type":"CustomPrice","context":"stockprice","asset":"nvda","currency":"usd"}
```

*queryData:*

```s
abi.encode("customprice", abi.encode("stockprice","nvda", "usd"))
```

`00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b637573746f6d70726963650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000000a73746f636b70726963650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000046e7664610000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0xa0b46dc2807f7a57ab0e8bbb07c3fab7d5f777855507967ae773fdd43d7c6455`
     
### Encoding/Decoding

A value of 99.9 would be submitted on-chain using the following bytes:

`0x0000000000000000000000000000000000000000000000056a6418b505860000`

## Dispute Considerations

Reporters should seek guidance from the intended data consumer when selecting data sources and choosing the algorithm to combine them.

- Multiple sources should be used whenever possible.
- When retrieving data directly from and api (exchanges), feed users might expect that exchanges with lower volumes
have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted.
