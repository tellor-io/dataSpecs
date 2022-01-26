# Spot Price Feeds

## Query Name

- `SpotPrice`

## Query Description

This query returns the spot price of an asset in a specified currency.

## Query Parameters

The `SpotPrice` query has two parameters, which specify the pricing pair.  

1. **asset** (string): Asset ID (e.g. BTC)
2. **currency** (string) Selected currency (e.g. USD)

To request new spot price pair, please reach out to the Tellor team or make an issue/PR in this repository

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


```s
    abi.encode("SpotPrice",abi.encode("btc","usd"))
```

`00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000953706f745072696365000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000003627463000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000`

*queryID:*

    `0xdac60eddde0c33db33b12e6adb2ea5b13d3572efe8fc757f81e8544453d5501a`

### TRB/USD Spot Price

*Query Descriptor:*

```json
{"type":"SpotPrice","asset":"trb","currency":"usd"}
```

```s
    abi.encode("SpotPrice",abi.encode("trb","usd"))
```

*queryID:*

     `0x168699163fbbbcf57c5ea0c0a960c7bc436372e23e498ea0eaf9d432b7f4ee6e`

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