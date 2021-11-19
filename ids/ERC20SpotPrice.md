# ERC-20 Token Spot Price Feeds

## Query Name

- `ERC20SpotPrice`

## Query Description

This query returns an ERC-20 token spot price in either USD or the native currency of the EVM chain.
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
    {"type":"ERC20SpotPrice","address":"0x88df592f8eb5d7bd38bfef7deb0fbc02cf3778a0","chain_id":1,"currency":"native"}
    ```

*queryID:*

    `0xa5e36e716292c57ff85d462666fa9064f2dd7f0f169cfb771d4b2c23c26ab944`

### TRB/USD Spot Price

*Query Descriptor:*

    ```json
    {"type":"ERC20SpotPrice","address":"0x88df592f8eb5d7bd38bfef7deb0fbc02cf3778a0","chain_id":1,"currency":"usd"}
    ```

*queryID:*

     `0x2c8932428647ecc49f97a319c2ebfc2f6e667cf8ee0e96d9eb1ebf1dbb777492`

## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes
have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted
