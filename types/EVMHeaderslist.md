## Type Name

`EVMHeaderslist`

## Description

The `EVMHeaderslist` query type allows users to submit a list of block hashes for a list blocks from any EVM compatible network and subsequently storing this information on the target chain.

## Query Parameters

The `EVMHeaderslist` query type's parameters are defined as:

```
1. chainId
    - description: This parameter corresponds to the unique identifier of the blockchain network.
    - value type: `uint256`
2. blockNumbers
    - description: This parameter represents a list of block numbers for which to submit block hashes for and they should be in descending order.
    - value type `uint256[]`
```

## Response Type

Response should return a list of bytes32 block header hashes for corresponding block numbers.

```
- abi_type: bytes32[]
- packed: false
```

## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first encode the parameter values in the order
specified above. Then encode those `bytes` with the Query's type string.

For example, to get the query data of an example instance of a `EVMHeaderslist` query using Solidity:

```s
bytes queryData = abi.encode("EVMHeaderslist", abi.encode(1, [17430128, 17430127, 17430126]));
```
Note: block numbers should be in descending order.

## Query ID

The Query ID is your new Query's unique identifier.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in
Solidity:

```s
bytes32 queryId = keccak256(queryData);
```

## JSON Representation

The JSON representation of your new query type is needed to construct query objects in a variety of languages. It
contains the essential components of your query: type name, parameters in an ordered list and their corresponding value
types, as well as the expected response type for the query.

the JSON representation of a `EVMHeaderslist` query:

```json
{
  "type": "EVMHeaderslist",
  "abi": [
    {
      "type": "uint256",
      "name": "chainId"
    },
    {
      "type": "uint256[]",
      "name": "blockNumber"
    }
  ],
  "response": {
    "type": "bytes32[]",
    "packed": false
  }
}
```

## Example

```s
bytes queryData = abi.encode("EVMHeaderslist", abi.encode(1, [17430128, 17430127, 17430126]));
bytes32 queryId = keccak256(queryData);
```

the queryData:
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000e45564d486561646572736c69737400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000003000000000000000000000000000000000000000000000000000000000109f670000000000000000000000000000000000000000000000000000000000109f66f000000000000000000000000000000000000000000000000000000000109f66e`

this queryId is `0x500434ad638b28b671225efdb086c0eae742f4c3e17bf8a4019d8538a43c52f9`

to format the response...

```s
bytes32[] exampleResponse = '0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000344086f750037db7f501cfaa2376284f303cc9cbcec36c3731a7d747870159e2644cb18d187a47004d889cdafd858a71ce79c7b1319ddfd84b2687afea72d9ec82b441e8e1579ab56cc5fd71bf32d096778ee7ef2c28fcba867766467c12e4607'
```

## Dispute Considerations
Its possible that a once truthful value could be invalid in case of block reorganizations and
in that case the dispute should resolve to _invalid_.
Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value
on-chain. Tellor is decentralized. This repo is a start to the education necessary for a fully decentralized oracle, but
please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage
monitoring and punishment of bad data.

## Suggested Data Sources

All the reporters need is a node and the `eth_call` RPC method!
