# Snapshot Specific Feeds

## Query Name

- `Snapshot`

## Query Description

This query returns the proposal result for a given proposalId (a specific proposal) coming from Snapshot.
See https://docs.snapshot.org/graphql-api for reference.

## Query Parameters

The `Snapshot` query has one parameter, which specifies the requested data.

1. **proposalId** (string): ID of the proposal.

The `proposalId` should be a valid proposal on Snapshot.

## Response Type

The query response will consist of a variable-length array of 256-bit values in the following format:

- `abi_type`: <uint256>[] (18 decimals of precision)
- `packed`: false

*Query Descriptor:*

```json
{
  "type": "Snapshot",
  "inputs": [
    {
      "type": "string",
      "name": "proposalId"
    }
  ],
  "outputs": [
    {
      "name": "SnapshotProposalValue",
      "decimals": 18,
      "type": "uint256[]",
      "packed": false
    }
  ]
}
```

*queryData:*

```s
abi.encode("Snapshot", abi.encode("aDd6cYVvfoKvkDX14jRcN86z6bfV135npUfhxmENjHnQ1"))
```

`0x000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000008536e617073686f7400000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000800000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000002d6144643663595676666f4b766b445831346a52634e38367a366266563133356e70556668786d454e6a486e513100000000000000000000000000000000000000`

_queryID:_

```s
keccak256(abi.encode("Snapshot",abi.encode("aDd6cYVvfoKvkDX14jRcN86z6bfV135npUfhxmENjHnQ1")))
```

`0x8ccceed53f0783c04efe8532b367c16a9ef9132e896f20f7946e38d72decf0a8`

### Encoding/Decoding

A value of [10023,1058] would be submitted on-chain using the following bytes:

`0x00000000000000000000000000000000000000000000000000000000000027270000000000000000000000000000000000000000000000000000000000000422`

## Dispute Considerations

- It is the reporters responsibility to ensure that the feed result is _reasonable_ enough for a community consensus, otherwise it may be subject to dispute.
- If a _reasonable_ value cannot be determined, a value should not be submitted

## Example

A working example can be found here: [https://gist.github.com/QuintusTheFifth/3c5d79c9dd5eb8edbb3fc55f5518de2b](https://gist.github.com/QuintusTheFifth/3c5d79c9dd5eb8edbb3fc55f5518de2b)
