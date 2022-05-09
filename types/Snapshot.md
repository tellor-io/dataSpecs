# Snapshot Specific Feeds

## Query Name

- `Snapshot`

## Query Description

This query returns the proposal result for a given proposal id (an IPFS hash for a certain proposal) coming from [Snapshot](https://snapshot.org/#/).
See https://snapshot.org/#/ for reference.

## Query Parameters

The `Snapshot` query has one parameter, which specifies the requested data.

1. **proposalId** (string): ID of the proposal.

The `proposalId` should be a valid proposal on Snapshot.

## Response Type

The query response will consist of a boolean value in the following format:

- `abi_type`: bool
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
      "type": "bool",
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

*queryID:*

```s
keccak256(abi.encode("Snapshot",abi.encode("aDd6cYVvfoKvkDX14jRcN86z6bfV135npUfhxmENjHnQ1")))
```

`0x8ccceed53f0783c04efe8532b367c16a9ef9132e896f20f7946e38d72decf0a8`

### Encoding/Decoding

A value of `true` would be submitted on-chain using the following bytes:

`0x0000000000000000000000000000000000000000000000000000000000000001`

## Dispute Considerations

- It is the reporters responsibility to return the correct result of a proposal, if a proposal succeeds a value of true is expected, if a proposal doesn't pass a value of false is submitted.


## Example

In a proposal with 10 000 yes votes and 2 000 no votes, a `true` value is expected to be returned to show that the proposal succeeded.
