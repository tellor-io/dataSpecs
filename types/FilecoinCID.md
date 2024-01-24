## Type Name

`FilecoinCID`


## Description

Filecoin IPFS users depend on Filecoin miners for reliable storage of their data, but verifying that the data is actually stored as expected is not always straightforward. This FilecoinCID query type aims to help recruit an unbiased third party (the decentralized Tellor reporter network) to report the generated CID for the data found at any provided URL.

## Query Parameters

The `FilecoinCID` query type has one parameter, `url`.

1. **Url**
    - description: The URL where some IPFS data can be found (e.g ```"https://bobswebsite.com/bobsresume.html"```)
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


## Example
A working example mapping of all the various inputs and parameters to a valid queryID. 

```s
queryData = abi.encode("FilecoinCID", abi.encode("https://ipfs.io/ipfs/QmZULkCELmmk5XNfCgTnCyFgAVxBRBXyDHGGMVoLFLiXEN"))
queryId = keccak256(queryData)
```

queryData:
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b46696c65636f696e43494400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000004368747470733a2f2f697066732e696f2f697066732f516d5a554c6b43454c6d6d6b35584e664367546e437946674156784252425879444847474d566f4c464c6958454e0000000000000000000000000000000000000000000000000000000000`

queryID:
`0x6934886e5ec915ab9f526832e9c026811d57c8ee4a6be1182c073a9d0fe132e7`

## Dispute Considerations

You can be disputed for reporting a CID that does not match the data found at the url in the requested (tipped) queryData.


## Suggested Data Sources

CIDs should be pulled form an api that you trust to generate CID hashes - OR - generated locally from downloaded files.
