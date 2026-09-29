## Type Name

`EVMBalanceCurrent`

## Description

The `EVMBalanceCurrent` query type allows users to query an EVM's native token balance by address. Users can tip Tellor reporters to bridge balances from any EVM chain to any tellor enabled chain.  

## Query Parameters

The `EVMBalanceCurrent` query type's parameters are defined as:
```
1. chainId
    - description: the chainId which you want the balance for
    - value type: `uint256`
2. evmAddress
    - description: the address of the hodler
    - value type: `address`
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

For example, to get the query data of an example instance of a `EVMBalanceCurrent` query using Solidity:
```s
address _addy = "0x0d7EFfEFdB084DfEB1621348c8C70cc4e871Eba4";
bytes queryData = abi.encode("EVMBalanceCurrent",abi.encode(1,_addy));
```

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```sol
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## JSON Representation
The JSON representation of your new query type is needed to construct query objects in a variety of languages. It contains the essential components of your query: type name, parameters in an ordered list and their corresponding value types, as well as the expected response type for the query.

the JSON representation of a `EVMBalanceCurrent` query:
```json
{
    "type": "EVMBalanceCurrent",
    "abi": [
        {
            "type": "uint256",
            "name": "chaindId",
        },{
            "type": "address",
            "name": "evmAddress",
        },
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

to query a mainnet Ethereum address:

```sol
bytes queryData = abi.encode("EVMBalanceCurrent", abi.encode(1,"0x0d7EFfEFdB084DfEB1621348c8C70cc4e871Eba4"));
bytes32 queryId = keccak256(queryData)
```

the queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000001145564d42616c616e636543757272656e74000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000010000000000000000000000000d7effefdb084dfeb1621348c8c70cc4e871eba4`

this queryId is `0x9b0b782ccf877909d2d9dc3e550350ae111e9f38322732335de641346b332cd4`

to format the response (0.38 ETH), pull out to 18 decimals, and use the timestamp of the block:

```sol
uint256 balance = 380000000000000000;
uint256 blockTimestamp = 1708099917;
bytes exampleResponse = abi.encode(balance, blockTimestamp);
```

this example response in bytes is...
`0x000000000000000000000000000000000000000000000000054607fc96a600000000000000000000000000000000000000000000000000000000000065cf894d`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- use a balance from 6 blocks ago to reduce the likelihood of the block being reverted or rolled back (you will be disputed)
- a dispute for a reported value which was correct at the time of the report but is no longer correct should resolve to invalid

## Suggested Data Sources

All the reporters need is an eth node or trusted explorer!
