## Type Name
- `LendingPairToxicity`

## Description

This query type is used to report a toxicity measure across multiple lending protocols of a given collateral/loan asset pair. The [formula for calculating this toxicity measure](./LendingPairToxicity/LendingPairToxicity.pdf) was co-authored by Pestopoppa who can be found on [Github](https://github.com/pestopoppa) and [Twitter](https://twitter.com/PestoPoppa).

## Query Parameters

The `LendingPairToxicity` query has two parameters, which specify the collateral and loan asset pair.

1. **collateral**
	- description: Collateral asset symbol (e.g. eth)
	- value type: `string`
2. **loan**
	- description: Loan asset symbol (e.g. dai)
	- value type: `string`


## Response Type
The query response will consist of a single 256-bit value in the following format:
- `abi_type`: ufixed256x18
- `packed`: false

## Query Data

- **Description:** Query data is used to form your Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects. To generate the query data, first encode the `collateral` and `loan` parameters as bytes, then encode the query type string with those bytes.
- **ABI Type:** `bytes`

For example, to get the query data of an example instance of a `LendingPairToxicity` query using Solidity:

```s
queryData = abi.encode(“LendingPairToxicity, abi.encode("eth", "dai"))
````

## Query ID
- **Description:** The unique identifier for this query is constructed by taking the Keccak hash of the query data bytes.
- **ABI Type:** `bytes32`

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```Solidity
queryId = keccak256(queryData)
```

You can use [this tool](#)(https://queryidbuilder.herokuapp.com/custom) to generate query IDs.

## JSON Representation
The JSON representation of your new query type is needed to construct query objects in a variety of languages. It contains the essential components of your query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

```
{
  "type": "LendingPairToxicity",
  "abi": [
    {
      "type": "string",
      "name": "collateral"
    },
	{
	  "type": "string",
	  "name": "loan"
	}
  ],
  "response": {
    "type": "ufixed256x18",
    "packed": false
  }
}
```

## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unnecessary disputes and at the same time encourage monitoring and punishment of bad data. 


## Suggested Data Sources

Calculating this toxicity measure depends on multiple on-chain and off-chain data points. The set of lending protocols used to calculate this measure is as follows: Hundred Finance and Aave. The set of on-chain token markets used to check liquidity is as follows: Balancer and QuickSwap. These sources will likely be updated in the future. Token prices should be derived from multiple sources.