## Type Name

`BTCBalance`


## Description

The `BTCBalance` query type allows users to query a BTC balance by address and timestamp. Users can tip Tellor reporters to bridge balances from Bitcoin to any tellor enabled chain.  


## Query Parameters

A query's parameters may change for each instance of your query type.

The `BTCBalance` query type's parameters are defined as:
```
1. btcAddress
    - description: the address of the bitcoin hodler
    - value type: `string`
2. timestamp
    - description: timestamp which will be rounded down to the closest Bitcoin block
    - value type: `uint256`
```

see [here](https://ethereum.stackexchange.com/questions/14037/what-is-msg-data) for more information on calldata

## Response Type

Response should return the value to 18 decimals

```
- abi_type: uint256
- packed: false
```


## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first UTF-8 encode the parameter values in the order specified above. Then encode those `bytes` with the Query's type string.

For example, to get the query data of an example instance of a `BTCBalance` query using Solidity:
```s
string btcAddress = "3Cyd2ExaAEoTzmLNyixJxBsJ4X16t1VePc";
bytes queryData = abi.encode("BTCBalance", abi.encode(btcAddress,1705954706));
```

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```s
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## JSON Representation
The JSON representation of your new query type is needed to construct query objects in a variety of languages. It contains the essential components of your query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

the JSON representation of a `BTCBalance` query:
```json
{
    "type": "BTCBalance",
    "abi": [
        {
            "type": "string",
            "name": "btcAddress",
        },
        {
            "type": "uint256",
            "name": "timestamp",
        },
    ],
    "response": {
        "type": "uint256",
        "packed": false,
    }
}
```


## Example

to query a bitcoin address at 22 January 2024 at 3:06pm EST

```s
bytes queryData = abi.encode("BTCBalance", abi.encode("3Cyd2ExaAEoTzmLNyixJxBsJ4X16t1VePc",1705954706));
bytes32 queryId = keccak256(queryData)
```

the queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000a42544342616c616e63650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000065aecd920000000000000000000000000000000000000000000000000000000000000022334379643245786141456f547a6d4c4e7969784a7842734a34583136743156655063000000000000000000000000000000000000000000000000000000000000`

this queryId is `0xee2cbdde34725cfa760bc074083ca85b3f82747d8f9b6baa79cecc2cb107c2a`

to format the response (.31 BTC), pull out to 18 decimals:

```s
bytes exampleResponse = abi.encode();
```

this example response in bytes is...
`0x000000000000000000000000000000000000000000000000044d575b885f0000`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- use timestamps old enough that block won't be reverted or rolled back (you will be disputed)

## Suggested Data Sources

All the reporters need is a btc node or trusted explorer!
