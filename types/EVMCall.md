## Type Name

`EVMCall`


## Description

The `EVMCall` query type allows users to bridge on-chain data from one EVM chain to another. Users can tip Tellor reporters to bridge balances, NFT ownership, and any other public data from one EVM blockchain to another, even if there is no bridge.


## Query Parameters

A query's parameters may change for each instance of your query type.

The `EVMCall` query type's parameters are defined as:
```
1. chainId
    - description: the chain id of the network the contract is deployed on
    - value type: `uint256`
2. contractAddress
    - description: the address of the contract on the network(e.g. 0x...)
    - value type: `address`
3. calldata
    - description: the encoding of the function name and its arguments
    - value type: `bytes`
```

see [here](https://ethereum.stackexchange.com/questions/14037/what-is-msg-data) for more information on calldata

## Response Type

Response should return a concatenation of the value and the timestamp

```
- abi_type: bytes
- packed: false
```


## Query Data

Query data is used to form your new Query's unique identifier, or query ID, and it's also included in emitted contract events so Tellor users and reporters can programmatically construct query objects.

To generate the query data for an instance of your new Query type, first UTF-8 encode the parameter values in the order specified above. Then encode those `bytes` with the Query's type string.

For example, to get the query data of an example instance of a `EVMCall` query using Solidity:
```s
bytes callDataExample = abi.encodeWithSignature("totalSupply()");
bytes queryData = abi.encode("EVMCall", abi.encode(1, 0x88dF592F8eb5D7Bd38bFeF7dEb0fBc02cf3778a0, callDataExample));
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

the JSON representation of a `EVMCall` query:
```json
{
    "type": "EVMCall",
    "abi": [
        {
            "type": "uint256",
            "name": "chainId",
        },
        {
            "type": "address",
            "name": "contractAddress",
        },
        {
            "type": "bytes",
            "name": "calldata",
        },
    ],
    "response": {
        "type": "bytes",
        "packed": false,
    }
}
```


## Example

to query Tellor's total supply on Ethereum mainnet:

```s
bytes callDataExample = abi.encodeWithSignature("totalSupply()");
bytes queryData = abi.encode("EVMCall", abi.encode(1, 0x88dF592F8eb5D7Bd38bFeF7dEb0fBc02cf3778a0, callDataExample));
bytes32 queryId = keccak256(queryData);
```

the queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000745564d43616c6c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000000000100000000000000000000000088df592f8eb5d7bd38bfef7deb0fbc02cf3778a00000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000418160ddd00000000000000000000000000000000000000000000000000000000`

this queryId is `0xd7472d51b2cd65a9c6b81da09854efdeeeff8afcda1a2934566f54b731a922f3`

to format the response...

```s
bytes exampleResponse = abi.encode(
    abi.encode(2390472032948139443578988), // Total supply of TRB returned from function call
    1654697834 // Timestamp of the block when the function call was made
);
```

this example response in bytes is...
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000062a0af6a000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000001fa33bfa23eb18dab686c`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- use the (Ethereum Signature Database)[https://www.4byte.directory/signatures/] to avoid mispelled function signatures
- use double quotes and no spaces if building function signatures from scratch
- make calls on an EVM compatible chain

## Suggested Data Sources

All the reporters need is a node and the `eth_call` RPC method!
