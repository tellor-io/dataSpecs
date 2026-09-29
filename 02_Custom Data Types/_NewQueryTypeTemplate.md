This is an example template for a new Query type. Every `Query` specification must include the following:

## Type Name

A unique name of your new Query type (e.g. `SpotPrice`, `Snapshot`). The markdown file you create based off this template will be named using the query's type name:

    <query-type-name>.md


## Description

A general description of the Query type, including its purpose and suggested use cases.


## Query Parameters

A query's parameters may change for each instance of your query type. For example, a `SpotPrice` query has both an `asset` and `currency` parameter. One instance of a `SpotPrice` query could have parameter values of `eth` (asset) and `usd` (currency), while another's `currency` parameter value might be `eur`. Query parameters should be styled in `mixedCase`, matching Solidity styling. The definition of your new query type's parameters must each include:
  - Parameter name (a string)
  - Description and value specification
  - Valid ABI type
  - The parameter order required for the query data bytes (more on this later)

For example, the `SpotPrice` query type's parameters are defined as:
```
1. asset
    - description: Asset ID (e.g. BTC)
    - value type: `string`
2. currency
    - description: Selected currency (e.g. USD)
    - value type: `string`
```

For a query with no parameters, `QueryWithNoParameters`, one should always include a single empty bytes argument with the name `phantom` as a convention:

```
1. phantom
   - description: Empty bytes, always used for query types with no arguments
   - value type: `bytes`
```


## Response Type

Specify the ABI type as a valid ETH ABI grammar string(e.g uint256, bytes32, bytes, etc), as well as a boolean signifying whether the reponse will be packed or not when encoded to bytes. More on valid types [here](https://docs.soliditylang.org/en/v0.8.13/types.html).

For example, the `SpotPrice`'s response type is an unpacked 256 bit value with 18 decimals of precision:
```
- abi_type: ufixed256x18 (18 decimals of precision)
- packed: false
```


## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first UTF-8 encode the parameter values in the order specified above. Then encode those `bytes` with the Query's type string. If there are no parameters for a query, instead encode empty bytes first.

For example, to get the query data of an example instance of a `SpotPrice` query using Solidity:
```s
queryData = abi.encode("SpotPrice", abi.encode("eth", "usd"))
```

And for a query with no parameters:
```s
queryData = abi.encode("QueryWithNoParameters", abi.encode(bytes("")))
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
    "type": "SpotPrice",
    "abi": [
        {
            "type": "string",
            "name": "asset",
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
A working example mapping of all the various inputs and parameters to a valid queryID. 


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 


## Suggested Data Sources

Where can reporters retrieve the query's response? Are there APIs available so reporters can programmatically retrieve the data for your query's expected response? Are there multiple possible sources for the expected response?
