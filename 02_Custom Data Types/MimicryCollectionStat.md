## Type Name

- `MimicryCollectionStat`

## Description

This query returns a metric derived from an NFT collection's sales data. Its initial use case is enabling the creation of markets on [Mimicry Protocol](https://www.mimicry.finance/), but the values can be used by anyone who needs one of the outlined metrics for a given NFT collection.

The metric itself can be one types outlined in the following list. Note that list will likely grow in the future, and as items are added, they will be documented here, and their respective `metric` value will be added below. In order to maintain data integrity, metric types will never be removed.

- Time-Adjusted Market Index (TAMI)
    - TAMI calculates the estimated value of a collection of assets with guards for price swings. It does so using the algorithm defined in [this repository](https://github.com/Mimicry-Protocol/TAMI).
    - Return value currency type: USD
- Market Cap
    - Market cap is calculated by summing the floor price or last sale value of each NFT (whichever is higher) in a collection.
    - Return value currency type: USD

## Query Parameters

The `MimicryCollectionStat` query has three parameters:

1. **chainId**
    - description: The chain id of the blockchain the collection lives on
    - value type: `uint256`
2. **collectionAddress**
    - description: The address of the NFT collection.
    - value type: `address`
3. **metric**
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
queryData = abi.encode("MimicryCollectionStat", abi.encode(1, "0x5180db8F5c931aaE63c74266b211F580155ecac8", 0))
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
    "type": "MimicryCollectionStat",
    "abi": [
        {
            "type": "uint256",
            "name": "chainId",
        },
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

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.12;
import "usingtellor/contracts/UsingTellor.sol";


contract Example is UsingTellor {

    constructor(address payable _tellorAddress) UsingTellor (_tellorAddress) public {}

    function getCryptoCovenTami() external view returns (uint256) {
        bytes memory _args = abi.encode(
            1,                                            // Ethereum chain id
            "0x5180db8F5c931aaE63c74266b211F580155ecac8", // Crypto Coven contract address
            0                                             // TAMI metric
        );
        bytes memory _queryData = abi.encode("MimicryCollectionStat", _args);
        bytes32 _queryId = keccak256(_queryData);

        (bytes memory value, uint256 timestampRetrieved) = getDataBefore(_queryId, block.timestamp - 1 hours);

        if (timestampRetrieved == 0) {
            return 0;
        }

        return abi.decode(value, (uint256));
    }
}
```

_queryData:
`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000154d696d69637279436f6c6c656374696f6e53746174000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002a30783531383064623846356339333161614536336337343236366232313146353830313535656361633800000000000000000000000000000000000000000000`

_queryId:
`0x9c0be0fdd6bfae597ed6e433543b50e8aa3963ba9d32cbf98b15c5533050fa4f`

## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 


## Suggested Data Sources

The Mimicry team is currently working on standing up a hosted API that will provide the metrics outlined above for any NFT collection. For transparency, the initial methods used to calculate the available metrics are listed below.

Note that this is an initial implementation of the system described, and it is subject to change in the future. For example, we plan on introducing filters such as wash trading and outlier detection to remove sales which have a high likelyhood of price manipulation.

- TAMI
    - Request 12 month historical sales data for a specific collection from [Reservoir](https://reservoir.tools/)
    - Process historical data based on [TAMI algorithm](https://github.com/Mimicry-Protocol/TAMI)
- Market Cap
    - Request all historical sales data for a specific collection from [Reservoir](https://reservoir.tools/)
    - For each token in the collection, calculate its value by taking the greater value between the collection's floor price and last sale price of that NFT
    - Sum the value of all items in the collection
