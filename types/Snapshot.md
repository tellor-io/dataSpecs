# Snapshot Specific Feeds

## Query Name

- `Snapshot`

## Query Description

This query returns the proposal result for a given proposalId (a specific proposal) coming from Snapshot

## Query Parameters

The `Snapshot` query has one parameter, which specifies the requested data.  

1. **proposalId** (string): ID of the proposal. 

The `proposalId` should be a valid proposal on Snapshot.

## Response Type

The query response will consist of a a variable-length array of 256-bit values in the following format:

- `abi_type`: <uint256>[] (18 decimals of precision)
- `packed`: false

*Query Descriptor:*

```json
    {
        "type": "Snapshot",
        "inputs": [{
            "type": "string",
            "name": "proposalId"
        }],
        "outputs": [
            {
              "name": "SnapshotProposalValue",
              "decimals": 18,
              "type": "uint256[]",
              "packed":false
            }
        ]
    }
```

*queryData:*

```s
abi.encode("Snapshot", abi.encode("1"))
```

`0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000008536e617073686f740000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000013100000000000000000000000000000000000000000000000000000000000000`

*queryID:*
```s
keccak256(abi.encode("Snapshot",abi.encode("1")))
```

`0xf671faa5e259eabb9c7eb72d17957ab3951b014f381ed1779bf72d6c0f8f48d4`

### Encoding/Decoding

A value of [10023,1058] would be submitted on-chain using the following bytes:

`0x00000000000000000000000000000000000000000000000000000000000027270000000000000000000000000000000000000000000000000000000000000422`


## Dispute Considerations

- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted
