## Type Name

`FilecoinCID`


## Description

Filecoin IPFS users depend on Filecoin miners for reliable storage of their data, but verifying that the data is actually stored as expected is not always straightforward. This FilecoinCID query type aims to help recruit an unbiased third party (the decentralized Tellor reporter network) to report a generated CID for the data found at any provided URL.

## Query Parameters

The `FilecoinCID` query type has one parameter, `url`.

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

To get the query data of an example instance of a `FilecoinCID` query using Solidity:
```s
queryData = abi.encode("FilecoinCID", abi.encode("https://github.com/tellor-io/dataSpecs/blob/FilecoinCID/types/SpotPrice.md"))
```


## Query ID

The Query ID is a Query's unique identifier.

To generate the query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). 
For example, in Solidity:
```s
keccak256(queryData)
```

You can use [this tool](https://tellor.io/queryidstation/) to generate query IDs.


## JSON Representation

For example, the JSON representation of a `FilecoinDealStatus` query:
```json
{
    "type": "FilecoinCID",
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


## Examples
A working example mapping of all the various inputs and parameters to a valid queryID. 

**for the url: "https://raw.githubusercontent.com/tellor-io/dataSpecs/main/README.md"**
```s
queryData = abi.encode("FilecoinCID", abi.encode("https://raw.githubusercontent.com/tellor-io/dataSpecs/main/README.md"))
queryId = keccak256(queryData)
```

queryData:
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b46696c65636f696e43494400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000004468747470733a2f2f7261772e67697468756275736572636f6e74656e742e636f6d2f74656c6c6f722d696f2f6461746153706563732f6d61696e2f524541444d452e6d6400000000000000000000000000000000000000000000000000000000`

queryID:
`0x7358f056230629915e1e6a8ef2c2f02496de3de644cbfadfced0449b784f8d47`


**Tellor reporter's response should be:**

The ipfs CID Value submitted as bytes:
`0x000000000000000000000000000000000000000000000000000000000000002e516d645734464e4b7350533979796b4b4e357343375135337064574a527a325458675652476e5a48617750377165000000000000000000000000000000000000`

Decoded Value:
`QmdW4FNKsPS9yykKN5sC7Q53pdWJRz2TXgVRGnZHawP7qe`

## Dispute Considerations

You can be disputed for reporting a CID that does not match the data found at the url in the requested (tipped) queryData.


## Suggested Data Sources

CIDs should be pulled form an api that you trust to generate CID hashes - OR - generated locally from downloaded files.
