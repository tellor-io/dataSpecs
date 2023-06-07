## Type Name

`EVMHeader`

## Description

The `EVMHeader` query type allows users to bridge block header information from any EVM compatible network and
subsequently storing this information on the target chain.

## Query Parameters

The `EVMHeader` query type's parameters are defined as:

```
1. chainId
    - description: This parameter corresponds to the unique identifier of the blockchain network.
    - value type: `uint256`
2. blockNumber
    - description: This parameter represents the specific number of the block for which the header information is to be retrieved.
    - value type `uint256`
```

## Response Type

Response should return the block header hash

```
- abi_type: bytes32
- packed: false
```

## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract
events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first UTF-8 encode the parameter values in the order
specified above. Then encode those `bytes` with the Query's type string.

For example, to get the query data of an example instance of a `EVMHeader` query using Solidity:

```s
bytes queryData = abi.encode("EVMHeader", abi.encode(1, 17403636));
```

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through
the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in
Solidity:

```s
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.

## JSON Representation

The JSON representation of your new query type is needed to construct query objects in a variety of languages. It
contains the essential components of your query: type name, parameters in an ordered list and their corresponding value
types, as well as the expected response type for the query.

the JSON representation of a `EVMHeader` query:

```json
{
  "type": "EVMHeader",
  "abi": [
    {
      "type": "uint256",
      "name": "chainId"
    },
    {
      "type": "uint256",
      "name": "blockNumber"
    }
  ],
  "response": {
    "type": "bytes32",
    "packed": false
  }
}
```

## Example

to query Tellor's total supply on Ethereum mainnet:

```s
bytes queryData = abi.encode("EVMHeader", abi.encode(1, 17403636));
bytes32 queryId = keccak256(queryData);
```

the queryData:
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000945564d4865616465720000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000001098ef4`

this queryId is `0xe095c6c5802bd7ec7eeaad48706373d60ce832dde0dfb239d35af931848acf00`

to format the response...

```s
bytes32 exampleResponse = '0x0b0b8824ab604536cc012d3124dff140c1ca39d032dcaa827fff69db63f5ebe3'
```

## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value
on-chain. Tellor is decentralized. This repo is a start to the education necessary for a fully decentralized oracle, but
please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage
monitoring and punishment of bad data.

## Suggested Data Sources

All the reporters need is a node and the `eth_call` RPC method!
