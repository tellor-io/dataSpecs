# Spot Price Feeds

## Query Name

- `SpotPrice`

## Query Description

This query returns the spot price of an asset in a specified currency.

## Query Parameters

The `SpotPrice` query has two parameters, which specify the pricing pair.  

1. **asset** (string): Asset ID (see [Asset List](data/assets.json))
2. **currency** (string) Selected currency

The `asset`/`currency` pair must be selected from the list of supported
spot price pairs located in the following file:

- [Spot Price Pairs](data/spot_price_pairs.json)

To request new spot price pair, please reach out to the Tellor team in discord, or
[submit an issue](https://github.com/tellor-io/telliot-core/issues),

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Examples

### BTC/USD Spot Price

*Query Descriptor:*

```json
{"type":"SpotPrice","asset":"btc","currency":"usd"}
```

*queryID:*

    `0xd66b36afdec822c56014e56f468dee7c7b082ed873aba0f7663ec7c6f25d2c0a`

### TRB/USD Spot Price

*Query Descriptor:*

```json
{"type":"SpotPrice","asset":"trb","currency":"usd"}
```

*queryID:*

     `0xd962318c0b96eaaa04e2e9a3ae518eb2986cdd97630c2d72fd107befb918d7dd`

### Encoding/Decoding

A value of 99.9 would be submitted on-chain using the following bytes:

    0x0000000000000000000000000000000000000000000000056ba3d73af34eec04


## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes
have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted