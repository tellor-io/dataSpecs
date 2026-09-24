# DIVA Protocol data specifications

## Query Name

- `DIVAProtocol`

## Query Description

This query returns the required information to settle a pool from a DIVA Protocol deployment on any chain.

### Information of Query:

For complete information, see the DIVA Tellor Oracle adapter docs: https://github.com/divaprotocol/oracles/blob/main/docs/Tellor.md

## Query Parameters

The `DIVAProtocol` query has three parameters, `poolId`, `divaAddress`, and `chainId`.

1. **poolId**
    - description: Unique identifier of the prediction market (e.g. `0x0fe386eff10c6903026ac911ea5e2d5076148a8f55aeea170f69a12e6da4353f`)
    - value type: `bytes32`
2. **divaAddress**
    - description: Contract address of DIVA Protocol containing the relevant pool information (`0x2C9c47E7d254e493f02acfB410864b9a86c28e1D` for all current networks; see [DIVA Protocol docs](https://github.com/divaprotocol/diva-protocol-v1/blob/main/DOCUMENTATION.md#contract-addresses))
    - value type: `address`
3. **chainId**
    - description: Network identifier (e.g. `137` for Polygon mainnet or `80001` for Mumbai testnet)

## Response Type

In DIVA Protocol, the reference asset can be any string, including asset pairs such as "ETH/USD" or "BTC/USD", or [URLs](https://bafybeielwtdfccwrpwcy2axaob3jcpbq5wqkh7qt7e64p3mhansme7lrie.ipfs.w3s.link/referenceAssetHotezRFK.json) that provide additional information on the event and reporting details.

The data query response will consist of a two floats represented as 256-bit integer values. The first float represents the outcome of the event, as defined by the reference asset and the expiry time. If the reference asset is an asset pair like "ETH/USD" or "BTC/USD" and no additional reporting details are provided, the value should be submitted as an integer with 18 decimals of precision. The second float represents the price of the collateral token in USD and is always represented as an integer with 18 decimals of precision. 

If there is no USD value available for the selected collateral token, please report the value as 0. In such cases, the settlement fee rewarded by DIVA Protocol will be fully credited to the excess fee recipient, while the reporter will receive nothing. It's important to note that in these instances, the reporter's only source of reward are tips provided by users.

Use DIVA Protocol's [`getPoolParameters`](https://github.com/divaprotocol/diva-protocol-v1/blob/main/DOCUMENTATION.md#getpoolparameters) function to retrieve the reference asset, the expiry time and the collateral token from the smart contract. Alternatively, query it from the [subgraph](https://github.com/divaprotocol/oracles/blob/main/docs/Tellor.md#contract-addresses-and-subgraphs).

- `abi_type`: `(ufixed256x18,ufixed256x18)`
- `packed`: false

An example of encoding and decoding this response type using Python:

```python
data = [2819.35, 0.9996]
submit_value = encode_abi(["uint256", "uint256"], [int(v * 1e18) for v in data])
print(submit_value)
```

_JSON Representation:_

```json
{
  "type": "DIVAProtocol",
  "abi": [
    {
      "type": "bytes32",
      "name": "poolId"
    },
    {
      "type": "address",
      "name": "divaAddress",
    },
    {
      "type": "uint256",
      "name": "chainId",
    }
  ],
  "response": {
    "type": "(ufixed256x18,ufixed256x18)",
    "packed": false
  }
}
```

_queryData:_

```s
abi.encode("DIVAProtocol", abi.encode(0x0fe386eff10c6903026ac911ea5e2d5076148a8f55aeea170f69a12e6da4353f, 0x2C9c47E7d254e493f02acfB410864b9a86c28e1D, 137))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000c4449564150726f746f636f6c000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600fe386eff10c6903026ac911ea5e2d5076148a8f55aeea170f69a12e6da4353f0000000000000000000000002c9c47e7d254e493f02acfb410864b9a86c28e1d0000000000000000000000000000000000000000000000000000000000000089`

_queryID:_

```s
keccak256(abi.encode("DIVAProtocol", abi.encode(0x0fe386eff10c6903026ac911ea5e2d5076148a8f55aeea170f69a12e6da4353f, 0x2C9c47E7d254e493f02acfB410864b9a86c28e1D, 137)))
```

`0xfc43126dbd1e3b7dc5ded0c6e4824786957558a71e1dd7adcedda51e1a4f188a`

The `queryData` and `queryId` can be obtained via the [getQueryDataAndId](https://github.com/divaprotocol/oracles/blob/main/docs/Tellor.md#getquerydataandid) function inside the [DIVA Tellor oracle adapter contract](https://github.com/divaprotocol/oracles/blob/main/docs/Tellor.md#contract-addresses-and-subgraphs).

### Encoding/Decoding

For example, if the reference asset is "ETH/USD" with a price of $2,819.35, and the collateral token is DAI at a price of $0.9996, then the hexadecimal of the bytes-encoded expected response for this query-- ready to be submitted by a reporter --is:

`0x000000000000000000000000000000000000000000000098d6574f71899000000000000000000000000000000000000000000000000000000ddf4ae7657b0000`

## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.

- The prediction market nature of DIVA Protocol means that the query could have a result that is a price, a temperature, a boolean, a sports score, or any different format for the data.
- The result should be the correct one AT THE TIME of expiration. This means that if the prediction market bets on the price of BTC/USD on Feb 1st at midnight, the query should always return the price of BTC/USD at midnight on Feb 1st, even if it get called/requested again.
- Multiple sources should be used whenever possible.
- When retrieving data directly from exchanges, feed users might expect that exchanges with lower volumes have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.
- It is the reporters responsibility to ensure that the feed result is _reasonable_ enough for a community consensus, otherwise it may be subject to dispute.
- If a _reasonable_ value cannot be determined, a value should not be submitted
