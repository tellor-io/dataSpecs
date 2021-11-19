# ERC-20 Token Spot Price in USD

## Query Name

 `ERC20SpotPriceUSD`

## Query Description

This query will return the spot price for an ERC-20 Token in US Dollars.
The token is uniquely identified by a contract address and an EVM chain ID.

## Query Parameters

| Name        | Data Type   |  Description                                 |
| ----------- | ----------- | -------------------------------------------- |
| `address`   | str         |  Contract address (beginning with `0x`)      |
| `chain_id`  | int         |  EVM Chain ID                                |

## Response Type

`abi_type`: `ufixed256x18` (18 decimals of precision)

`packed`: `false`

## Example

The following is an example query descriptor for the TRB spot price on mainnet:

`{"type":"ERC20SpotPrice","address":0x88df592f8eb5d7bd38bfef7deb0fbc02cf3778a0","chain_id",1}`

## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes
have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted
