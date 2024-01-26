## Type Name

`EVMBalance`


## Description

The `EVMBalance` query type allows users to query a an EVM's native token balance by address and timestamp. Users can tip Tellor reporters to bridge balances from any EVM chain to any tellor enabled chain.  


## Query Parameters

A query's parameters may change for each instance of your query type.

The `EVMBalance` query type's parameters are defined as:
```
1. chainId
    - description: the chainId which you want the balance for
    - value type: `uint256`
2. evmAddress
    - description: the address of the hodler
    - value type: `address`
3. timestamp
    - description: timestamp which will be rounded down to the closest Ethereum block
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

For example, to get the query data of an example instance of a `EVMBalance` query using Solidity:
```s
adddress _addy = "0x0d7EFfEFdB084DfEB1621348c8C70cc4e871Eba4";
bytes queryData = abi.encode("EVMBalance",abi.encode(1,_addy,1705954706));
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

the JSON representation of a `EVMalance` query:
```json
{
    "type": "EVMBalance",
    "abi": [
        {
            "type": "uint256",
            "name": "chaindId",
        },{
            "type": "address",
            "name": "evmAddress",
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

to query a mainnet Ethereum address at 22 January 2024 at 3:06pm EST

```s
bytes queryData = abi.encode("EVMBalance", abi.encode(1,"0x0d7EFfEFdB084DfEB1621348c8C70cc4e871Eba4",1705954706));
bytes32 queryId = keccak256(queryData)
```

the queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000a45564d42616c616e636500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000d7effefdb084dfeb1621348c8c70cc4e871eba40000000000000000000000000000000000000000000000000000000065aecd92`

this queryId is `0xca5aae1e155dcaa848c3dd85ecb799a65fdc0628a586c843a5b352eab74cb2b8`

to format the response (0.43287710009898539 ETH), pull out to 18 decimals:

```s
bytes exampleResponse = abi.encode();
```

this example response in bytes is...
`0x0000000000000000000000000000000000000000000000000601e36dd6cd1dae`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- use timestamps old enough that block won't be reverted or rolled back (you will be disputed)

## Suggested Data Sources

All the reporters need is an eth node or trusted explorer!
