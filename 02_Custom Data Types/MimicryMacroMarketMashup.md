## Type Name

- `MimicryMacroMarketMashup`


## Description

This query returns a single market capitalization metric derived from the sum of zero or more NFT collection market capitialization metrics, plus zero or more ERC-20 market capitalization metrics.

Its initial use case is enabling the creation of macro-markets on [Mimicry Protocol](https://www.mimicry.org/), but the values can be used by anyone who needs this metric.

The intent of this Data Spec is to continue to expand with more and more asset classes.


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

1. **metric**
    - description: The metric that should be use for all calculations. 99.9% of the time this will always be "market-cap"
    - value type: `string`

2. **currency**
    - description: The currency that should be used for all calculations. 99.9% of the time this will always be "usd"
    - value type: `string`

3. **collections**
    - description: An array of Collection structs, where each Collection includes the blockchain where the NFT contract is deployed, and the contract address itself 
    - value type: `Collection[]`
    - struct: `tuple(string, address)`

4. **tokens**
    - description: An array of Token structs, where each Token includes the blockchain where the token contract is deployed, the lower-case short name of the token, and the token contract address itself
    - value type: `Token[]`
    - struct: `tuple(string, string, address)`

> Note that in the future we may include NFT collections and tokens from more chains than "ethereum-mainnet".

## Response Type

The response will consist of a single 256-bit value that reprents the summed market capitalization of all requested assets, represented in USD, rounded to the nearest whole dollar.

- `abi_type`: uint256
- `packed`: false


## Query Data and ID Example

Please see the example below request the market capitalization of the all Sandbox, Decentraland, and Otherdeed land NFTs on the Ethereum Mainnet, along with the market capitalization of all circulating SAND, MANA, and APE tokens, represented in USD. You can test this by pasting the following code into https://remix.ethereum.org/.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.17;

struct Collection {
    string chainName;
    address collectionAddress;
}

struct Token {
    string chainName;
    string tokenName;
    address tokenAddress;
}

contract Example {

    function getQueryData() 
        public 
        pure 
        returns (bytes memory) 
    {
        Collection[] memory _collections = new Collection[](3); // length of 3 is arbitrary and must match use case
        Token[] memory _tokens = new Token[](3);                // length of 3 is arbitrary and must match use case
        // Token[] memory _tokens = new Token[](0);             // example... use length of 0 when an asset type is not needed
        
        _collections[0] = Collection("ethereum-mainnet", 0x50f5474724e0Ee42D9a4e711ccFB275809Fd6d4a);   // sandbox land
        _collections[1] = Collection("ethereum-mainnet", 0xF87E31492Faf9A91B02Ee0dEAAd50d51d56D5d4d);   // decentraland land
        _collections[2] = Collection("ethereum-mainnet", 0x34d85c9CDeB23FA97cb08333b511ac86E1C4E258);   // otherdeed land

        _tokens[0] = Token("ethereum-mainnet", "sand", 0x3845badAde8e6dFF049820680d1F14bD3903a5d0);     // sand token
        _tokens[1] = Token("ethereum-mainnet", "mana", 0x0F5D2fB29fb7d3CFeE444a200298f468908cC942);     // mana token
        _tokens[2] = Token("ethereum-mainnet", "ape", 0x4d224452801ACEd8B2F0aebE155379bb5D594381);      // ape token

        bytes memory _args = abi.encode(
            "market-cap",   // 99.9999999% of the time this will not change
            "usd",          // 99.9999999% of the time this will not change
            _collections,   // this param will always be an array of Collection
            _tokens         // this param will always be an array of Token
        );
        return abi.encode("MimicryMacroMarketMashup", _args);
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
            string memory _metric, 
            string memory _currency, 
            Collection[] memory _collections, 
            Token[] memory _tokens
        )
    {
        (_metric, _currency, _collections, _tokens) = abi.decode(data, (string, string, Collection[], Token[]));
    }
}
```

**getQueryData():**
`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000184d696d696372794d6163726f4d61726b65744d617368757000000000000000000000000000000000000000000000000000000000000000000000000000000620000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000300000000000000000000000000000000000000000000000000000000000000000a6d61726b65742d63617000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000375736400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000160000000000000000000000000000000000000000000000000000000000000004000000000000000000000000050f5474724e0ee42d9a4e711ccfb275809fd6d4a0000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e6574000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000040000000000000000000000000f87e31492faf9a91b02ee0deaad50d51d56d5d4d0000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e657400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000034d85c9cdeb23fa97cb08333b511ac86e1c4e2580000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e6574000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000001400000000000000000000000000000000000000000000000000000000000000220000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000003845badade8e6dff049820680d1f14bd3903a5d00000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e657400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000473616e6400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000f5d2fb29fb7d3cfee444a200298f468908cc9420000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e65740000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000046d616e6100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000004d224452801aced8b2f0aebe155379bb5d5943810000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e65740000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000036170650000000000000000000000000000000000000000000000000000000000`

**getQueryId(queryData):**
`0x30717c1fa75f5bae0c11e13283bedcf72e9f4e526c642bd5b330b615d0db8708`

**decodeArgs(data):**

`0: string: _metric market-cap`

`1: string: _currency usd`

`2: tuple(string,address)[]: _collections ethereum-mainnet,0x50f5474724e0Ee42D9a4e711ccFB275809Fd6d4a,ethereum-mainnet,0xF87E31492Faf9A91B02Ee0dEAAd50d51d56D5d4d,ethereum-mainnet,0x34d85c9CDeB23FA97cb08333b511ac86E1C4E258`

`3: tuple(string,string,address)[]: _tokens ethereum-mainnet,sand,0x3845badAde8e6dFF049820680d1F14bD3903a5d0,ethereum-mainnet,mana,0x0F5D2fB29fb7d3CFeE444a200298f468908cC942,ethereum-mainnet,ape,0x4d224452801ACEd8B2F0aebE155379bb5D594381`


## JSON Representation

```json
{
    "type": "MimicryMacroMarketMashup",
    "abi": [
        {
            "type": "string",
            "name": "_metric",
        },
        {
            "type": "string",
            "name": "_currency",
        },
        {
            "type": "Collection[]",     // tuple(string, address)
            "name": "_collections",
        },
        {
            "type": "Token[]",          // tuple(string, string, address)
            "name": "_tokens",
        },
    ],
    "response": {
        "type": "uint256",
        "packed": false,
    }
}
```

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


## Comprehensive Calculation Example

You may test the calculation for this metric using [this public api endpoint](https://runkit.io/aslangoldenhour/macro-market-mashup/branches/master?queryData=0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000184d696d696372794d6163726f4d61726b65744d617368757000000000000000000000000000000000000000000000000000000000000000000000000000000620000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000300000000000000000000000000000000000000000000000000000000000000000a6d61726b65742d63617000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000375736400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000e00000000000000000000000000000000000000000000000000000000000000160000000000000000000000000000000000000000000000000000000000000004000000000000000000000000050f5474724e0ee42d9a4e711ccfb275809fd6d4a0000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e6574000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000040000000000000000000000000f87e31492faf9a91b02ee0deaad50d51d56d5d4d0000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e657400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000034d85c9cdeb23fa97cb08333b511ac86e1c4e2580000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e6574000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000001400000000000000000000000000000000000000000000000000000000000000220000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000003845badade8e6dff049820680d1f14bd3903a5d00000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e657400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000473616e6400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000f5d2fb29fb7d3cfee444a200298f468908cc9420000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e65740000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000046d616e6100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000004d224452801aced8b2f0aebe155379bb5d5943810000000000000000000000000000000000000000000000000000000000000010657468657265756d2d6d61696e6e65740000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000036170650000000000000000000000000000000000000000000000000000000000). 

[Here's the source code](https://runkit.com/aslangoldenhour/macro-market-mashup) for this endpoint.