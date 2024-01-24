## Type Name

`FilecoinCID`


## Description

Filecoin IPFS users depend on Filecoin miners for reliable storage of their data, but verifying that the data is actually stored as expected is not always straightforward. This FilecoinCID query type aims to help recruit an unbiased third party (the decentralized Tellor reporter network) to report the generated CID for the data found at any provided URL.

## Query Parameters

The `FilecoinCID` query type has one parameter, `url`.

1. **Url**
    - description: The URL where some data can be found (e.g ```"https://bobswebsite.com/bobsresume.html"```)
    - value type: `string`


## Response Type

Response should return the abi encoded CID for the data found at the url.

```
- abi_type: string
- packed: false
```

## Query Data

To get the query data of an example instance of a `FilecoinCID` query using Solidity:
```s
queryData = abi.encode("FilecoinCID", abi.encode("https://bobswebsite.com/bobsresume.html"))
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

**for https://bobswebsite.com/bobsresume.html:**
```s
queryData = abi.encode("FilecoinCID", abi.encode("https://github.com/tellor-io/dataSpecs/blob/FilecoinCID/types/SpotPrice.md"))
queryId = keccak256(queryData)
```

queryData:
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b46696c65636f696e43494400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000004a68747470733a2f2f6769746875622e636f6d2f74656c6c6f722d696f2f6461746153706563732f626c6f622f46696c65636f696e4349442f74797065732f53706f7450726963652e6d6400000000000000000000000000000000000000000000`

queryID:
`0x48cdfe04f99283e7e62bc0366e3b423e582ca1aecf9eae037bdd00e31b5f9559`


**Tellor Response (at time of writing) for SpotPrice.md:**

CID for SpotPrice.md: 
`QmVKYyYhSmSCciuApLbmPXwxk7teTKNK8xtpAAf3CeviSw`

Value submitted as bytes:
`0x000000000000000000000000000000000000000000000000000000000000002e516d564b59795968536d534363697541704c626d505877786b377465544b4e4b3878747041416633436576695377000000000000000000000000000000000000`


## Dispute Considerations

You can be disputed for reporting a CID that does not match the data found at the url in the requested (tipped) queryData.


## Suggested Data Sources

CIDs should be pulled form an api that you trust to generate CID hashes - OR - generated locally from downloaded files.
