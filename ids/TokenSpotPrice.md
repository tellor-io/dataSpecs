# Token Spot Price Feeds

## Query Name

- `TokenSpotPrice`

## Query Description

This query returns an token spot price in either USD or the native currency of the EVM chain.
The token is uniquely identified by a contract address and an EVM chain ID.

## Query Parameters

This query accepts 3 parameters, in the following order.  All parameters are mandatory.

1. **address** (string): Token contract address
2. **chain_id** (integer): Token chain ID
3. **currency** (string) Selected currency, one of the following values:
   - `native`: returns the price in the native currency of the chain (e.g. ETH for chain_id = 1)
   - `usd`: returns the price in USD

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Examples

### TRB/ETH Spot Price

*Query Descriptor:*

```json
{"type":"TokenSpotPrice","address":"0x88df592f8eb5d7bd38bfef7deb0fbc02cf3778a0","chain_id":1,"currency":"native"}
```

*queryID:*

    `0xb77c3f01bfbf486f4bdc5e5aae007ebbdaf47dc7cb0572d6a2cc473ad957f69a`

### TRB/USD Spot Price

*Query Descriptor:*

```json
{"type":"TokenSpotPrice","address":"0x88df592f8eb5d7bd38bfef7deb0fbc02cf3778a0","chain_id":1,"currency":"usd"}
```

*queryID:*

     `0xc83928d2035b30eb2ed0fa81b1dd45051360813e46f6a91fcdefe11a0653250c`

## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes
have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted
