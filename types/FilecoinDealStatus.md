## Type Name


`FilecoinDealStatus`


## Description

This is a proposal for a new query type for reporting Filecoin Deal Statuses. A Filecoin Deal is a special type of smart contract on Filecoin that exchanges FIL tokens for an active provision of IPFS storage. Given a unique ID called a ProposalCID, a data reporter can determine if a paid agreement to provide storage with Filecoin (on IPFS) is still active. Users can use this query type to create EVM smart contract incentives on Filecoin deals that are more complex than the deals themselves.

## Query Parameters

```
1. proposalCID
    - description: unique ID of Filecoin storage deal
    - value type: `string`
```


## Response Type

```
- abi_type: bool
- packed: false
```


## Query Data

For example, to get the query data of an example instance of a `FilecoinDealStatus` query using Solidity:
```s
queryData = abi.encode("FileCoinDealStatus", abi.encode("bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"))
```


## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```s
keccak256(queryData)
```

You can use [this tool](https://queryidbuilder.herokuapp.com/custom) to generate query IDs.


## JSON Representation

For example, the JSON representation of a `FilecoinDealStatus` query:
```json
{
    "type": "FilecoinDealStatus",
    "abi": [
        {
            "type": "string",
            "name": "proposalCID",
        },
    ],
    "response": {
        "type": "bool",
        "packed": false,
    }
}
```


## Example
A working example mapping of all the various inputs and parameters to a valid queryID. 

```s
queryData = abi.encode("FileCoinDealStatus", abi.encode("bafy2bzacea3wsdh6y3a36tb3skempjoxqpuyompjbmfeyf34fi3uy6uue42v4"))
queryId = keccak256(queryData)
```

queryData:
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000001246696c65636f696e4465616c537461747573000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000003e6261667932627a616365613377736468367933613336746233736b656d706a6f78717075796f6d706a626d666579663334666933757936757565343276340000`

queryData:
`0xa192d70f9a5e804503362ed2c51e11b4432c6d432fd658a82b0d0ec18a98fe2b`

## Dispute Considerations

you can be disputed for:
- reporting the status of the wrong deal
- reporting an invalid response
- lying about the deal status (can be reviewed by others using APIs or filecoin block explorers)


make sure to:
- double check the proposalCID string
- double check the proposalCID is valid
- double check the proposalCID 


## Suggested Data Sources

Where can reporters retrieve the query's response? Are there APIs available so reporters can programmatically retrieve the data for your query's expected response? Are there multiple possible sources for the expected response?

You can call the Filecoin `ClientGetDealUpdates` and `ClientGetDealStatus` endpoints with...

- Glif hosted node
- Lotus local node
- Infura API
