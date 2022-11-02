## Type Name

- `MimicryCollectionMetric`

## Description

This query returns a metric derived from an NFT collection's sales data. Its initial use case is enabling the creation of markets on [Mimicry Protocol](https://www.mimicry.finance/), but the values can be used by anyone who needs one of the outlined metrics for a given NFT collection.

The metric itself can be one types outlined in the following list. 

- Time-Adjusted Market Index (TAMI)
    - TAMI calculates the estimated value of a collection of assets with guards for price swings and time . It does do using the algorithm defined in [this repository](https://github.com/Mimicry-Protocol/TAMI).
    - Return value currency type: USD
- Market Cap
    - Market cap is calculated by summing the last sale value or floor price (whichever is higher) of each NFT in a collection.
    - Return value currency type: USD

## Query Parameters

The `MimicryCollectionMetric` query has two parameters:

1. **collectionAddress**
    - description: The address of the NFT collection.
    - value type: `address`
2. **metric**
    - description: An integer representing the type of metric being requested.
    - value type: `uint256`
    - allowed values:
        - `0` - TAMI
        - `1` - Market Cap

## Response Type

The response will consist of a single 256-bit value with 18 decimals of precision. The currenty type of the response is determined by the `metric` that it is returning.

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Query Data

Example queryData to request the TAMI of the [Crypto Coven](https://etherscan.io/address/0x5180db8f5c931aae63c74266b211f580155ecac8) collection:

```s
queryData = abi.encode("MimicryCollectionMetric", abi.encode("0x5180db8F5c931aaE63c74266b211F580155ecac8", 0))
```

## Query ID

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:

```s
keccak256(queryData)
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## JSON Representation

```json
{
    "type": "MimicryCollectionMetric",
    "abi": [
        {
            "type": "address",
            "name": "collectionAddress",
        },
        {
            "type": "uint256",
            "name": "metric",
        },
    ],
    "response": {
        "type": "ufixed256x18",
        "packed": false,
    }
}
```

## Example

Full example to request Crypto Coven's TAMI:

```s
queryData = abi.encode("MimicryCollectionMetric", abi.encode("0x5180db8F5c931aaE63c74266b211F580155ecac8", 0))
queryId = keccack256(queryData)
```

queryData:
`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000174d696d69637279436f6c6c656374696f6e4d657472696300000000000000000000000000000000000000000000000000000000000000000000000000000000400000000000000000000000005180db8f5c931aae63c74266b211f580155ecac80000000000000000000000000000000000000000000000000000000000000000`

queryId:
`0x746bc37ea8ea58074c817d3e67e10cbea833d969ff2fa4831ea064c2f2de990d`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 


## Suggested Data Sources

The Mimicry team is currently working on standing up a hosted API that will provide the metrics outlined above for any NFT collection. For transparency, the initial methods used to calculate the available metrics are listed below.

Note that this is an initial implementation of the system described, and it is subject to change in the future. 

- TAMI
    - Request 12 month historical sales data for a specific collection from [Reservoir](https://reservoir.tools/)
    - Process historical data based on [TAMI algorithm](https://github.com/Mimicry-Protocol/TAMI)
- Market Cap
    - Request collection market cap for a specific collection from [NFTGo](https://nftgo.io/)
