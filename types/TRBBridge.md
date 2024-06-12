## Type Name

`TRBBridge`


## Description

The `TRBBridge` query type allows users to bridge the TRB from Ethereum to Tellor Layer. The data type works by digesting a deposit ID and direction (bool whether you are going to Layer or not).  The deposit ID corresponds to the deposit ID associated with the deposit on the TRB bridge contract on each chain.  The data type will then return the ETH address, cosmos address, and amount.  


## Query Parameters

A query's parameters may change for each instance of your query type.

The `TRBBridge` query type's parameters are defined as:
```
1. toLayer
    - description: true if going from Ethereum to Layer, false if going the opposite direction
    - value type: `bool`
2. depositId
    - description: the correspoinding depositId of each transaction
    - value type: `uint256`

```

## Response Type

Response should return abi-encoded bytes correspoinding to the eth Address, layer address, and amount.  The parameters are as follows:

1. ethAddress
    - description: the Ethereum address either doing the deposit (if toLayer) or recieving the funds otherwise
    - value type: `address`
2. layerAddress 
    - description: the layer address (i.e. cosmos sdk address) either doing the deposit or recieving the funds (if toLayer)
    - value type: `string`
3. amount
    - description: the amount of tokens being bridged
    - value type: `uint256`



## Query Data


To get the query data of an example instance of a `TRBBridge` query using Solidity:
```s
bytes queryData = abi.encode("TRBBridge", abi.encode(true,9));
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

the JSON representation of a `TRBBridge` query:
```json
{
    "type": "TRBBridge",
    "abi": [
        {
            "type": "bool",
            "name": "toLayer",
        },
        {
            "type": "uint256",
            "name": "depositId",
        },
    ],
    "response": {
        "type": "(address,address,uint256)",
        "packed": false,
    }
}
```


## Example

to query a deposit event from Ethereum to Layer of 68 TRB corresponding to deposit ID 421:

```s
bytes queryData = abi.encode("TRBBridge", abi.encode(true,421));
bytes32 queryId = keccak256(queryData);
```

the queryData: `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000954524242726964676500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000001a5`

this queryId is `0x9f7d83f1e1acbb2b714b424cb14bc7d94fe507d4c1202181345adb88f209504d`

to format the response...

```s
bytes exampleResponse = abi.encode(
    abi.encode(0x0d7EFfEFdB084DfEB1621348c8C70cc4e871Eba4, // eth address
    tellor12z5sdp7ayjwshr3c93scrn4x0v2lmlmzemt73j, // layer address
    68000000000000000000 //68 TRB
);
```

this example response in bytes is...
`0000000000000000000000000d7effefdb084dfeb1621348c8c70cc4e871eba40000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000003afb087b876900000000000000000000000000000000000000000000000000000000000000000002d74656c6c6f7231327a357364703761796a77736872336339337363726e34783076326c6d6c6d7a656d7437336a00000000000000000000000000000000000000`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- use a finalized block.  If Ethereum or Layer rolls back, you will be subject to a dispute
- again, do not report a block that has not been finalized
- use valid addresses.  A wrong address as the destinaton can result in loss of funds

## Suggested Data Sources

All the reporters need is a node and the `eth_call` RPC method!
