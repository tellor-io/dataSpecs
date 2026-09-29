## Type Name

`StringQuery`


## Description

The `StringQuery` query type allows users to enter arbitrary questions and receive an answer in the form of a string. It acts as a catch-all to allow for quick proto-typing, or possibly even production use. 


## Query Parameters

A query's parameters may change for each instance of your query type.

The `StringQuery` query type's parameters are defined as:
```
1. text
    - description: arbitrary query or question
    - value type: `string`
```

## Response Type

Response should return a string which answers the text question.  Unless specified in the question, all responses should be camel case. 

```
- abi_type: string
- packed: false
```


## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first UTF-8 encode the parameter values in the order specified above. Then encode those `bytes` with the Query's type string.

For example, to get the query data of an example instance of a `StringQuery` query using Solidity:
```s
string memory _queryText = "What is the capital of France?";
bytes memory _queryDataArgs = abi.encode(_queryText);
bytes memory _queryData = abi.encode("StringQuery", _queryDataArgs);
```

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```s
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://tellor.io/queryidstation/) to generate query IDs.


## JSON Representation
The JSON representation of your new query type is needed to construct query objects in a variety of languages. It contains the essential components of your query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

the JSON representation of a `StringQuery` query:
```json
{
    "type": "StringQuery",
    "abi": [
        {
            "type": "string",
            "name": "text",
        },
    ],
    "response": {
        "type": "string",
        "packed": false,
    }
}
```


## Example

To query the capital of France:

```s
string memory _queryText = "What is the capital of France?";
bytes memory _queryDataArgs = abi.encode(_queryText);
bytes memory _queryData = abi.encode("StringQuery", _queryDataArgs);
bytes32 _queryId = keccak256(_queryData);
```

the queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b537472696e67517565727900000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000001e5768617420697320746865206361706974616c206f66204672616e63653f0000`

this queryId is `0xd5f3ae01cc1c4fd97d99b0996570a03ec5ec1078187280fb173057367cec8efa`

to format the response...

```s
string memory _responseString = "Paris";
bytes memory _exampleResponse = abi.encode(_responseString);
```

this example response in bytes is...
`0x000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000055061726973000000000000000000000000000000000000000000000000000000`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- Make query text as clear as possible.
- Try to answer the query with a response as precise and concise as possible.

## Suggested Data Sources

As this query type supports any arbitrary string query, the list of possible data sources is endless and depends on each specific query. 
