## Type Name

`FileCID`


## Description

Filecoin / IPFS users depend on miners for reliable storage of their data, but verifying that the data is actually stored as expected is not always straightforward. This FileCID query type aims to help recruit an unbiased third party (the decentralized Tellor reporter network) to report a correctly generated CID for the data found at any provided URL.

## Query Parameters

The `FileCID` query type has one parameter, `url`.

1. **Url**
    - description: The url for the data (e.g ```"https://raw.githubusercontent.com/tellor-io/dataSpecs/main/README.md"```)
    - value type: `string`


## Response Type

Response should return an abi encoded CID Value matching the data found at the url.

```
- abi_type: string
- packed: false
```

## Query Data

To get the query data of an example instance of a `FileCID` query using Solidity:
```s
queryData = abi.encode("FileCID", abi.encode("https://raw.githubusercontent.com/tellor-io/dataSpecs/main/README.md"))
```
Encoded queryData:
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000746696c654349440000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000004468747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f74656c6c6f722d696f2f6461746153706563732f6d61696e2f524541444d452e6d6400000000000000000000000000000000000000000000000000000000`

## Query ID

The Query ID is a Query's unique identifier.

To generate the query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). 
For example, in Solidity:
```s
keccak256(queryData)
```

You can use [this tool](https://tellor.io/queryidstation/) to generate query IDs.

Encoded queryID:
`0x81c2d4d0f826cc936f6bfc110120445d648b9f6ab815984420598bea60b416ca`

## JSON Representation

For example, the JSON representation of a `FilecoinDealStatus` query:
```json
{
    "type": "FileCID",
    "abi": [
        {
            "type": "string",
            "name": "url",
        },
    ],
    "response": {
        "type": "string",
        "packed": false,
    }
}
```


## Example Reporter Response

If a user creates a tip transaction using the queryData and queryID shown above for the url [here](https://raw.githubusercontent.com/tellor-io/dataSpecs/main/README.md)

The Tellor reporter should report the ipfs CID Value submitted as bytes:
`0x000000000000000000000000000000000000000000000000000000000000002e516d645734464e4b7350533979796b4b4e357343375135337064574a527a325458675652476e5a48617750377165000000000000000000000000000000000000`

Decoded Value:
`QmdW4FNKsPS9yykKN5sC7Q53pdWJRz2TXgVRGnZHawP7qe`

**Note: Actual CID for example data may change if dataSpecs readme changes.*

## Dispute Considerations

You can be disputed for reporting a CID that does not match the data found at the url in the requested (tipped) queryData. Reporters should test their clients thoroughly on a supported testnet before submitting for FileCID queries on mainnet.

## Suggested Data Sources

CIDs should be pulled form an api that you trust to generate CID hashes - OR - generated locally.
