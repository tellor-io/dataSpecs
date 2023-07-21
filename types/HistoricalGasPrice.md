
## Type Name

`HistoricalGasPrice`


## Description

This is a query type for reporting a gas price in gwei to the Tellor network. The query type will be called `HistoricalGasPrice. An Example use case is for gas refunds: protocols will need an accurate estimate for the cost of historical user transactions in gas. Given its approach, this `HistoricalGasPrice` is best suited for refunding users an approximation of their gas spent according to an average of gas prices from that time period.


## Query Parameters

The parameters of the `HistoricalGasPrice` query type will be the `chainId` and `timestamp`

```
1. chainId
    - description: unique id of the EVM chain (ex: rinkeby is 4)
    - value type: `uint256`
2. timestamp
    - description: the unix timestamp to calculate on-chain gas price at
    - value type: `uint256`
```


## Response Type

`HistoricalGasPrice`'s response type is an unpacked 256 bit value with 18 decimals of precision. It's response type is measured in gwei:
```
- abi_type: ufixed256x18 (18 decimals of precision)
- packed: false
```


## Query Data

```s
queryData = abi.encode("HistoricalGasPrice", abi.encode(1, 1650465649))
```

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:

```s
keccak256(queryData)
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## JSON Representation
The JSON representation of a `HistoricalGasPrice` query:
```json
{
    "type": "HistoricalGasPrice",
    "abi": [
        {
            "type": "uint256",
            "name": "chainId",
        },
        {
            "type": "uint256",
            "name": "timestamp",
        },
    ],
    "response": {
        "type": "ufixed256x18",
        "packed": false,
    }
}
```


## Example
A working example mapping of all the various inputs and parameters to a valid queryID. 

The queryData for mainnet ethereum (chainId `1`) at unix timestamp `1650465649`:

```s
queryData = abi.encode("HistoricalGasPrice", abi.encode(1, 1650465649))
queryId = keccack256(queryData)
```

queryData: `0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000012486973746f726963616c47617350726963650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000062601b71`

queryId:
`0xf451dd40a7d42f120c488d1b27d148b3deb1293d22bd7378e524e12d0a659584`

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

**don't do this**
- reporting a gas price from one historical transaction. gas prices are very volatile
- reporting the current gas price from metamask. gas prices are very volatile
- do not report the base fee and priority fee separately

**do this**
- aggregate/medianize gas prices from the relevant block(s)
- be careful when sourcing data from a black box API
- use EIP-1559 gas strategy (the sum of the base fee and priority fee)
- always measured in gwei



## Suggested Data Sources

Oracle provides a "gas price history" API. It aggregates gas prices between timestamps, which can be used to calculate a median. It supports the following networks:
- Ethereum
- BSC
- Fantom
- Polygon
- Cronos
- Harmony
- Celo
- Heco
- Moonriver
- Fuse
