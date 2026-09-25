## Type Name

- `MimicryNFTMarketIndex`


## Description

This query returns a market capitalization metric derived from the 50 most valuable NFT collections' sales data on a particular chain. 

Its initial use case is enabling the creation of a macro-market on [Mimicry Protocol](https://www.mimicry.org/), but the values can be used by anyone who needs this metric to infer the directional market sentiment of traders of NFTs.

### Market Capitalization, Explained
Market capitalization is calculated as the sum of each NFT within the 50 most valuable NFT collections, where each NFT is valued at the greater of its last traded price or the floor price of the collection, respectively.

#### For ERC721 collections:
- Market Cap = the sum of all NFT values of the collection
- The value of each NFT = max(last price, floor price)

#### For ERC1155 collections:
- Market Cap = the sum of all NFT values of the collection
- The value of each NFT = max(last price, floor price) times the number of owners

Suspected wash trades should be filtered out.

> Note that this market capitalization methodology has been heavily influenced by NFTGo's [collection metrics documentation](https://docs.nftgo.io/docs/collection-metrics). A visualization of the top 50 most valuable collections, according to NFTGo, is available [here](https://nftgo.io/discover/top-collections?timeSpan=all&sort=marketCapEth). 


## Query Parameters

The `MimicryNFTMarketIndex` query has two parameters:

1. **chain**
    - description: The blockchain mainnet that the collections live on (e.g. ethereum, solana, etc.)
    - value type: `string`
2. **currency**
    - description: The currency used to calculate the market capitalization (e.g. usd, eth, sol)
    - value type: `string`

> Note that in time we expect this query to support more blockchains and/or currencies.

## Response Type

The response will consist of a single 256-bit value with 18 decimals of precision.

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false


## Query Data

Example queryData to request the market capitalization of the 50 most valuable NFT collections on the Ethereum Mainnet, represented in USD:

```s
queryData = abi.encode("MimicryNFTMarketIndex", abi.encode('ethereum', 'usd'))
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
    "type": "MimicryNFTMarketIndex",
    "abi": [
        {
            "type": "string",
            "name": "chain",
        },
        {
            "type": "string",
            "name": "currency",
        },
    ],
    "response": {
        "type": "ufixed256x18",
        "packed": false,
    }
}
```


## Example

Full example to request the market capitalization of the 50 most valuable NFT collections on the Ethereum Mainnet, represented in USD:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.17;

contract NFTMarketIndex {

    function getQueryData() 
        public 
        pure 
        returns (bytes memory) 
    {
        bytes memory _args = abi.encode(
            "ethereum",   // 99.9999999% of the time this will not change
            "usd"         // 99.9999999% of the time this will not change
        );
        return abi.encode("MimicryNFTMarketIndex", _args);
    }
    
    function getQueryId(bytes calldata queryData) external pure returns (bytes32) {
        return keccak256(queryData);
    }

    function decodeQueryData(bytes calldata data) 
        external 
        pure 
        returns (string memory _name, bytes memory _args) 
    {
        (_name, _args) = abi.decode(data, (string, bytes));
    }

    function decodeArgs(bytes calldata data) 
        external 
        pure 
        returns (
            string memory _chain, 
            string memory _currency
        )
    {
        (_chain, _currency) = abi.decode(data, (string, string));
    }

    function decodeQueryValue(bytes calldata _value)
        external
        pure
        returns (uint256) 
    {
        return abi.decode(_value, (uint256));
    }
}
```

_queryData:
`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000154d696d696372794e46544d61726b6574496e646578000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000008657468657265756d00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000`

_queryId:
`0x486e2149f25d46bb39a27f5e0c81be9b6f193abf46c0d49314b8d1dd104cd53b`



## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

### Special Note on Currencies
Reporting in a novel currency is a not as simple as taking the pre-calculated market capitalization metric from NFTGo in ETH and looking up some other currency's current spot price. The reason is because the pre-calculated values (from both NFTGo and Banksea Finance) take into account the spot price of ETH and SOL at the time of each NFT's last sale transaction. So in order to support a currency such as LINK, one must look at the price of LINK at the time of each sale, convert to that value, and then keep a running total of the sum of NFT sales. 


## Suggested Data Sources

### NFTGo (Ethereum)
The easiest way to calculate this metric is using the [Retrieve Collection Ranking](https://docs.nftgo.io/reference/get_collection_ranking_eth_v1_market_rank_collection__time_range__get-1) API endpoint provided by NFTGo, as follows:

```bash
curl --request GET \
     --url 'https://data-api.nftgo.io/eth/v1/market/rank/collection/all?by=market_cap&with_rarity=false&asc=false&offset=0&limit=50' \
     --header 'X-API-KEY: YOUR-FREE-API-KEY' \
     --header 'accept: application/json'
```

Using this data source is as simple as iterating through the provided responses from the query above and summing the return values provided in either of the following two fields:

```json
{
  "collections": [
    {
      "market_cap_usd": 1823183682.9378865,
      "market_cap_eth": 884823.2714163727
    }
  ]
}
```

> Note that at the time of writing this endpoint is free to use. You can generate your NFTGo API key [here](https://developer.nftgo.io/login).

### Banksea Finance (Solana)
Another way to calculate this metric is using the [Collection API](https://banksea-finance.gitbook.io/banksea-oracle-api/api/solana-api/collection-api) endpoint provided by Banksea Finance.

The recommended approach is to first [get a list of supported NFT collections](https://banksea-finance.gitbook.io/banksea-oracle-api/api/solana-api/collection-api#huo-qu-yu-suan-ke-mu-v2), as follows:

```bash
curl -X "GET" "https://oracle-api.banksea.finance/nft/v1/collection/list" \
     -H 'x-api-key: test-api-key' \
     -H 'Content-Type: application/json; charset=utf-8'
```

Then to [get each NFT collection's market cap](https://banksea-finance.gitbook.io/banksea-oracle-api/api/solana-api/collection-api#huo-qu-yu-suan-ke-mu-v2-3), as follows:

```bash
curl -X "POST" "https://oracle-api.banksea.finance/nft/v1/collection/market_cap" \
     -H 'x-api-key: test-api-key' \
     -H 'Content-Type: application/json; charset=utf-8' \
     -d $'{
    "collection":"degods"
}'
```

And then, finally, to sum the top 50 most valuable NFT collections using the `market_cap` parameter in the response objects. 

```json
{
    "data": {
        "market_cap": 1398.0378111110,
    }
}
```

> Note that you can request a Banksea Finance API key [here](https://banksea-finance.gitbook.io/banksea-oracle-api/cooperation/request-form) and [here](https://c2dtw7wmuwa.typeform.com/to/DBQ3SUXv).

### Other Sources
The more difficult way to calcuate this metric is by sourcing sales history from Reservoir Protocol, Dune Analytics, etc. and manually calculating the market capitalization per the methodology described herein. 


## Comprehensive Calculation Example

You may test the calculation for this metric using [this public api endpoint](https://runkit.io/aslangoldenhour/calculate-nft-market-index-via-nftgo/branches/master?queryData=0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000154d696d696372794e46544d61726b6574496e646578000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000008657468657265756d00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000). 

[Here's the source code](https://runkit.com/aslangoldenhour/calculate-nft-market-index-via-nftgo) for this endpoint.