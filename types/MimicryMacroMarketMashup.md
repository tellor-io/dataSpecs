## Type Name

- `MimicryMacroMarketMashup`


## Description

This query returns a single market capitalization metric derived from the sum of zero or more NFT collection market capitialization metrics, plus zero or more ERC-20 market capitalization metrics.

Its initial use case is enabling the creation of macro-markets on [Mimicry Protocol](https://www.mimicry.org/), but the values can be used by anyone who needs this metric.

### Market Capitalization for NFT Collections, Explained
Market capitalization is calculated as the sum of each NFT within an NFT collection, where each NFT is valued at the greater of its last traded price or the floor price of the collection, respectively.

#### For ERC721 collections:
- Market Cap = the sum of all NFT values of the collection
- The value of each NFT = max(last price, floor price)

#### For ERC1155 collections:
- Market Cap = the sum of all NFT values of the collection
- The value of each NFT = max(last price, floor price) times the number of owners

Suspected wash trades should be filtered out.

> Note that this market capitalization methodology has been heavily influenced by NFTGo's [collection metrics documentation](https://docs.nftgo.io/docs/collection-metrics). 

### Market Capitalization for ERC-20 Tokens, Explained
The market capitalization of an ERC-20 token is the total market value of a cryptocurrency's circulating supply. It is analogous to the free-float capitalization in the stock market.

- Market Cap = Current Price * Circulating Supply


## Query Parameters

The `MimicryMacroMarketMashup` query has two parameters:

1. **ethereumNftCollections**
    - description: A CSV list of NFT collection contract-addresses on the Ethereum Mainnet
    - value type: `string`
3. **tokens**
    - description: A CSV list of cryptocurrency tokens (e.g. 'APE, MANA, SAND')
    - value type: `string`

> Note that no currency is specified because the resulting market capitalization will always be returned in USD. 

> Also note that in the future we may include NFT collections from more chains (hence the query parameter naming), as well as more asset classes (e.g. stocks, commodities, fine spirits, etc.).

## Response Type

The response will consist of a single 256-bit value that reprents the summed market capitalization of all requested assets, represented in USD, rounded to the nearest whole dollar.

- `abi_type`: uint256
- `packed`: false


## Query Data

Example queryData to request the market capitalization of the all Sandbox, Decentraland, and Otherdeed land NFTs on the Ethereum Mainnet, along with the market capitalization of all circulating SAND, MANA, and APE tokens:

```javascript
queryData = abi.encode("MimicryMacroMarketMashup", abi.encode("0x50f5474724e0Ee42D9a4e711ccFB275809Fd6d4a,0xf87e31492faf9a91b02ee0deaad50d51d56d5d4d,0x34d85c9CDeB23FA97cb08333b511ac86E1C4E258", "SAND,MANA,APE"))
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
    "type": "MimicryMacroMarketMashup",
    "abi": [
        {
            "type": "string",
            "name": "ethereumNftCollections",
        },
        {
            "type": "string",
            "name": "solanaNftCollections",
        },
        {
            "type": "string",
            "name": "tokens",
        },
    ],
    "response": {
        "type": "uint256",
        "packed": false,
    }
}
```


## Example

Full example to request the market capitalization of all land NFTs from Sandbox, Decentraland, and Otherdeed, plus the market capitalization of the SAND, MANA, and APE tokens, represented in USD:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.12;
import "usingtellor/contracts/UsingTellor.sol";
contract Example is UsingTellor {
    constructor(address payable _tellorAddress) UsingTellor (_tellorAddress) public {}
    function getMetaverseMarketCap() external view returns (uint256) {
        bytes memory _args = abi.encode(
            "0x50f5474724e0Ee42D9a4e711ccFB275809Fd6d4a,0xf87e31492faf9a91b02ee0deaad50d51d56d5d4d,0x34d85c9CDeB23FA97cb08333b511ac86E1C4E258",
            "SAND,MANA,APE"
        );
        bytes memory _queryData = abi.encode("MimicryMacroMarketMashup", _args);
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
`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000174d696d696372794d6163726f4d61726b6574496e6465780000000000000000000000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000e000000000000000000000000000000000000000000000000000000000000000803078353066353437343732346530456534324439613465373131636346423237353830394664366434612c3078663837653331343932666166396139316230326565306465616164353064353164353664356434642c307833346438356339434465423233464139376362303833333362353131616338364531433445323538000000000000000000000000000000000000000000000000000000000000000d53414e442c4d414e412c41504500000000000000000000000000000000000000`

_queryId:
`0xfe4fff3585418f5d0b6c3dd3ce92cba1227904d158a97d7cef5363fd656517a3`



## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized. This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 


## Suggested Data Sources

### For NFT Collection Market Capitalization Metrics

#### **NFTGo (Ethereum)**
The easiest way to calculate the market capitalization for an NFT collection on the Ethereum blockchain is using the [Retrieve Collection Metrics](https://docs.nftgo.io/reference/get_metrics_eth_v1_collection__contract_address__metrics_get-1) API endpoint provided by NFTGo, as follows:

```bash
curl --request GET \
     --url https://data-api.nftgo.io/eth/v1/collection/{contract-address}/metrics \
     --header 'X-API-KEY: YOUR-API-KEY' \
     --header 'accept: application/json'
```

Using this data source is as simple as taking the value of the `market_cap_usd` field:

```json
{
    "market_cap_usd": 502517289.9482534
}
```

> Note that at the time of writing this endpoint is free to use. You can generate your NFTGo API key [here](https://developer.nftgo.io/login).


#### **Reservoir Protocol (Ethereum)**
The next easiest way to calculate the market capitalization for an NFT collection on the Ethereum blockchain is using the [Sales](https://docs.reservoir.tools/reference/getsalesv4) API endpoints provided by Reservoir Protocol, as follows:

```bash
curl --request GET \
     --url 'https://api.reservoir.tools/sales/v4?contract={contract-address}' \
     --header 'accept: */*' \
     --header 'x-api-key: demo-api-key'
```

You can iterate through every response to find the latest sale price for each NFT in a collection, as shown below:

```json
{
  "sales": [
    {
      "token": {
        "tokenId": "106308",
      },
      "price": {
        "amount": {
          "usd": 4175.77693,
        }
      },
      "washTradingScore": 0
    }
  ]
}
```

> Note that you can discover a collection's floor price using the [Collections](https://docs.reservoir.tools/reference/getcollectionsv5) API endpoint from Reservoir Protocol, or the [Retrieve Collection Metrics](https://docs.nftgo.io/reference/get_metrics_eth_v1_collection__contract_address__metrics_get-1) API endpoint from NFTGo, among others. If you use Reservoir's *Collections* endpoint you must convert the floor price to USD from ETH.


#### **Other Sources**
NFT collection market capitalization metrics may also be calculated by sourcing sales history from Dune Analytics, a custom Subgraph, etc.

In all cases other than NFTGo, one must manually calculate the market capitalization per the methodology described herein.



### For ERC-20 Market Capitalization Metrics

There are many sources that report the market capitalization of ERC-20 tokens, including but not limited to:
- [CoinMarketCap's Quote Latest v2](https://coinmarketcap.com/api/documentation/v1/#operation/getV2CryptocurrencyQuotesLatest)
- [CoinGecko's /simple/price API v3](https://www.coingecko.com/en/api/documentation)
