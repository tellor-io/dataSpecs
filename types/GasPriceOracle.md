This is an example template for a new Query type. Every `Query` specification must include the following:


## Type Name

`Gas Price Oracle`


## Description

This is a proposal for a new query type for reporting a gas price to the Tellor network. The query type will be called `GasPriceOracle`. Its main use case is for gas refunds: protocols will need an accurate estimate for the cost of historical user transactions in gas. Given its approach, this `GasPriceOracle` is best suited for refunding users an approximation of their gas spent according to an average of gas prices from that time period.


## Query Parameters

The parameters of the `GasPriceOracle` query type will be the `chaindId` and `timestamp`

```
1. chainId
    - description: unique id of the EVM chain (ex: rinkeby is 4)
    - value type: `uint256`
2. timestamp
    - description: the unix timestamp to calculate on-chain gas price at
    - value type: `uint256`
```


## Response Type

`GasPriceOracle`'s response type is an unpacked 256 bit value with 18 decimals of precision:
```
- abi_type: ufixed256x18 (18 decimals of precision)
- packed: false
```


## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first UTF-8 encode the parameter values in the order specified above. Then encode those `bytes` with the Query's type string.

For example, to get the query data of an example instance of a `GasPriceOracle` query using Solidity:
```s
queryData = abi.encode("GasPriceOracle", abi.encode("1", "1650465649"))
```

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```s
keccak256(queryData)
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## JSON Representation
The JSON representation of your new query type is needed to construct query objects in a variety of languages. It contains the essential components of your query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

For example, the JSON representation of a `SpotPrice` query:
```json
{
    "type": "GasPriceOracle",
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

The queryData for mainnet ethereum at unix timestamp `1650465649`:

```s
queryData = abi.encode("GasPriceOracle", abi.encode("1", "1650465649"))
queryId = keccack256(queryData)
```

queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000e47617350726963654f7261636c65000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000062601b71`

queryId:
`0x7dd07b75e8c096ea1926e9a2cc8f749b04581d0ae88a892989f05790384344f6`

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

- retreiving a gas price from one historical transaction is not recommended. gas price is very volatile
- retreiving the current gas price from metamask is not recommended. gas price is very volatile
- care should be used when reporting from black box aggregators
- use EIP-1559 gas strategy



## Suggested Data Sources

Where can reporters retrieve the query's response? Are there APIs available so reporters can programmatically retrieve the data for your query's expected response? Are there multiple possible sources for the expected response?

Owlracle privode's a "gas price history" API. It supports the following networks:
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