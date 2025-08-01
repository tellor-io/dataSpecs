## Type Name

`TRBBridge`


## Description

The `TRBBridge` query type allows users to bridge the TRB token between Ethereum mainnet and Tellor Layer. The data type works by indicating a deposit ID and direction (`bool toLayer` whether you are going to Tellor Layer or not). The deposit ID is associated with the deposit (or withdrawal) in the TRB bridge on each of the two chains.  The data type will then return the ETH address, layer address, total amount, and tip amount. Bridging from Ethereum to Layer is considered a deposit, and bridging from Layer to Ethereum is considered a withdrawal. 

## Query Parameters

A query's parameters may change for each instance of a query type.

The `TRBBridge` query type's parameters are defined as:
```
1. toLayer
    - description: true if going from Ethereum to Layer, false if going the opposite direction
    - value type: `bool`
2. depositId
    - description: the corresponding deposit ID of each transaction
    - value type: `uint256`
```

## Response Type

Response should return abi-encoded bytes corresponding to the eth address, layer address, amount, and tip amount.  The parameters are as follows:

1. ethAddress
    - description: the Ethereum address either doing the deposit (if toLayer) or receiving the funds otherwise
    - value type: `address`
2. layerAddress 
    - description: the layer address (i.e. cosmos-sdk address) either receiving the deposit (if toLayer) or doing the withdrawal otherwise
    - value type: `string`
3. amount
    - description: the total amount of tokens being bridged
    - value type: `uint256`
4. tip
    - description: this parameter is required to be included in the response definition, but otherwise ignored by the bridge
    - value type: `uint256`


## Query Data


To get the query data of an example instance of a `TRBBridge` deposit query using Solidity:
```s
bool toLayer = true;
uint256 depositId = 9;
bytes queryData = abi.encode("TRBBridge", abi.encode(toLayer,depositId));
```

## Query ID

The Query ID is a query's unique identifier. It's important because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```s
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://querybuilder.tellor.io/) to generate query IDs.


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
        "type": "(address,string,uint256,uint256)",
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

```solidity
bytes exampleResponse = abi.encode(
    0x0d7EFfEFdB084DfEB1621348c8C70cc4e871Eba4, // eth address
    "tellor12z5sdp7ayjwshr3c93scrn4x0v2lmlmzemt73j", // layer address string
    68000000000000000000, // 68 TRB total amount bridged
    0 // 0 TRB tip
);
```

this example response in bytes is...
`0x0000000000000000000000000d7effefdb084dfeb1621348c8c70cc4e871eba40000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000003afb087b8769000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002d74656c6c6f7231327a357364703761796a77736872336339337363726e34783076326c6d6c6d7a656d7437336a00000000000000000000000000000000000000`


## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

Make sure to...
- wait until at least 100 ethereum blocks have been built on top of the deposit transaction's block before reporting
- again, do not report a block that has not been finalized
- use valid addresses.  A wrong address as the destinaton can result in loss of funds

## Suggested Data Sources

All the reporters need is a node and the `eth_call` RPC method! 

Deposit info should be read from the TRB token bridge contract on Ethereum. Only deposits from the TRB token bridge contract on Ethereum mainnet (evm chain id 1) should be reported to tellor mainnet (cosmos chain id tellor-1). The Ethereum mainnet bridge contract is deployed at address [0x5589e306b1920F009979a50B88caE32aecD471E4](https://etherscan.io/address/0x5589e306b1920f009979a50b88cae32aecd471e4). 

Deposit info can be read from the `deposits` function of the TRB token bridge contract on Ethereum.

```sol
struct DepositDetails {
    address sender;
    string recipient;
    uint256 amount;
    uint256 tip;
    uint256 blockHeight;
}

function deposits(uint256 _depositId) external view returns (DepositDetails memory);
```

Deposit information is also available via `Deposit` event logs.

```sol
event Deposit(uint256 _depositId, address _sender, string _recipient, uint256 _amount, uint256 _tip);
```