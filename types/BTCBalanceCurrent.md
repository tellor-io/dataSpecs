## Type Name

`BTCBalanceCurrent`


## Description

The `BTCBalanceCurrent` query type allows users to query a current BTC balance by address. Users can tip Tellor reporters to bridge balances from Bitcoin to any tellor enabled chain.  


## Query Parameters

A query's parameters may change for each instance of your query type.

The `BTCBalanceCurrent` query type's parameters are defined as:
```
1. btcAddress
    - description: the address of the bitcoin hodler
    - value type: `string`
```

see [here](https://ethereum.stackexchange.com/questions/14037/what-is-msg-data) for more information on calldata

## Response Type

Response should return the balance value to 18 decimals:

```
1. balance
    - abi_type: ufixed256x18
    - packed: false
2. blockTimestamp
    - abi_type: uint256
    - packed: false
```


## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first UTF-8 encode the parameter values in the order specified above. Then encode those `bytes` with the Query's type string.

For example, to get the query data of an example instance of a `BTCBalanceCurrent` query using Solidity:
```s
string btcAddress = "3Cyd2ExaAEoTzmLNyixJxBsJ4X16t1VePc";
bytes queryData = abi.encode("BTCBalanceCurrent", abi.encode(btcAddress));
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

the JSON representation of a `BTCBalanceCurrent` query:
```json
{
    "type": "BTCBalanceCurrent",
    "abi": [
        {
            "type": "string",
            "name": "btcAddress",
        }
    ],
    "response": [
        {
            "type": "ufixed256x18",
            "name": "balance",
            "packed": false,
        },
        {
            "type": "uint256",
            "name": "blockTimestamp",
            "packed": false,
        }
    ]
}
```


## Example

to query a current bitcoin address balance:

```sol
bytes queryData = abi.encode("BTCBalanceCurrent", abi.encode("3Cyd2ExaAEoTzmLNyixJxBsJ4X16t1VePc"));
bytes32 queryId = keccak256(queryData)
```

the queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000001142544342616c616e636543757272656e74000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000022334379643245786141456f547a6d4c4e7969784a7842734a34583136743156655063000000000000000000000000000000000000000000000000000000000000`

this queryId is `0xb24d33114098acd76e336b0bb1839d0c60169f3bf73a4ad9eea2cefb3b8a9b9a`

to format the response (.31 BTC), pull out to 18 decimals, and use the timestamp of the block:

```sol
uint256 balance = 310000000000000000
uint256 blockTimestamp = 1708099219
bytes exampleResponse = abi.encode(balance, blockTimestamp);
```

this example response in bytes is...
`0x000000000000000000000000000000000000000000000000044d575b885f00000000000000000000000000000000000000000000000000000000000065cf8693`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- use a balance from 6 blocks ago to reduce the likelihood of the block being reverted or rolled back (you will be disputed)
- a dispute for a reported value which was correct at the time of the report but is no longer correct should resolve to invalid

## Suggested Data Sources

All the reporters need is a btc node or trusted explorer. 
