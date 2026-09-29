## Type Name

`DLCProofOfReserve`


## Description

The DLC Proof Of Reserve query type is for reporting the valueLocked sum across all funded DLC vaults on a network where the DLCManager.sol contract is deployed. All funded vaults returned from the DLCManager contract then provide the data needed to retrieve the funding transaction on bitcoin for a vault and verify that the amount received on Bitcoin accurately reflects the valueLocked field of a DLC vault.

## Query Parameters

The `DLCProofOfReserve` query type has one parameter, `chain_id`.

1. **chain_id**
    - description: The chain id where you want to the proof of reserve to be verified (e.g ```42161```).
    - value type: `uint256`


## Response Type

Response should return an abi encoded sum of the valueLocked field for all verified funded vaults on the desired chain id

```
- abi_type: ufixed256x8 (8 decimals of precision)
- packed: false
```

## Query Data

To get the query data of an example instance of a `DLCProofOfReserve` query using Solidity:
```s
queryData = abi.encode("DLCProofOfReserve", abi.encode(42161))
```
Encoded queryData:
`0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000011444c4350726f6f664f66526573657276650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000a4b1`

## Query ID

The Query ID is a Query's unique identifier.

To generate the query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). 
For example, in Solidity:
```s
keccak256(queryData)
```

You can use [this tool](https://tellor.io/queryidstation/) to generate query IDs.

Encoded queryID:
`0xcd6144bd5bcc47f0a84a7d6e1055050beb1acebd261caaffe282c714ff8511a0`

## JSON Representation

For example, the JSON representation of a `DLCProofOfReserve` query:
```json
{
    "type": "DLCProofOfReserve",
    "abi": [
        {
            "type": "uint256",
            "name": "chain_id",
        },
    ],
    "response": {
        "type": "ufixed256x8",
        "packed": false,
    }
}
```


## Example Reporter Response

If a user creates a tip transaction using the queryData and queryID shown above for the url [here](https://raw.githubusercontent.com/tellor-io/dataSpecs/main/README.md)

The Tellor reporter should report the encoded sum of the value locked in the vaults:
`0x0000000000000000000000000000000000000000000000000000000055c9d290`

Decoded Value:
`1439290000`

**Note: Actual returned sum for example data may change if dataSpecs readme changes.*

## Dispute Considerations

Reports should be generated using the methodology outlined here (``https://docs.google.com/document/d/1E8pOe8W3SPmFLfmxBgCObDODWkVsKHt-RwUX-GA_bEI/edit``), otherwise they may be subject to dispute

## Suggested Data Sources

Please look here for an in depth walk through on how the value should be calculated: ``https://docs.google.com/document/d/1E8pOe8W3SPmFLfmxBgCObDODWkVsKHt-RwUX-GA_bEI/edit``
