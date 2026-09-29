# TellorRNG Feeds

## Query Name

- `TellorRNG`

## Query Description

This query returns a pseudorandom number by finding the next bitcoin blockhash after a given timestamp, finding the most recent ethereum blockhash before the bitcoin block, and hashing the two blockhashes together.

## Query Parameters

The `TellorRNG` query has one parameter which specifies the timestamp after which to find the next bitcoin blockhash.

1. **timestamp** (uint256): timestamp after which to take next bitcoin blockhash

## Response Type

The query response consists of a bytes32 value in the following format:

- `abi_type`: bytes32
- `packed`: false

*Query Descriptor:*

```json
{
  "type": "TellorRNG",
  "inputs": [
    {
      "type": "uint256",
      "name": "timestamp"
    }
  ],
  "outputs": [
    {
      "type": "bytes32",
      "packed": false
    }
  ]
}
```

*queryData:*

```s
abi.encode("TellorRNG", abi.encode(1652075943))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000954656c6c6f72524e4700000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000006278ada7`

*queryID:*

```s
keccak256(abi.encode("TellorRNG",abi.encode(1652075943)))
```

`0xd3329783230f1d05be5c25a6a26d19f6a6f0e0518747de5245608eb1afcaa58d`

### Encoding/Decoding

A value of `0xa588d598625d216c6f2416efa20970007c501b65a2a815d35389e92d7b944d92` would be submitted on-chain using the following bytes:

`0xa588d598625d216c6f2416efa20970007c501b65a2a815d35389e92d7b944d92`

## Dispute Considerations

- It is the reporters responsibility to return the correct result as specified in this document.
- If a reporter submits a correct random number but a chain reorganization later invalidates the reporter's submission, the dispute should resolve to _invalid_.


## Example

When requesting a random number for a timestamp of `1652075943`, one should first find the next bitcoin blockhash. Then find the most recent ethereum blockhash before that bitcoin block. The next bitcoin block is number `735571` with a blockhash of `0000000000000000000201fec7d610dd93645f677dfe68afdd1a3b534194e6af`. The most recent ethereum block before the bitcoin block is number `14740820` with a blockhash of `0xaf3454d3f72c9d3e7b2c26887f6dd1b89431b872e8c6a2c329ee64c8744edf59`. These blockhashes should be converted to strings, encoded with packing into bytes, and hashed using `keccak256` to get a final pseudorandom number of `0x9d6946d952f13e7125134611ad9f5dcc4108d90359b02394d8ef6769cc8574b3`.

```solidity
bytes32 randomNumber = keccak256(abi.encodePacked('0xaf3454d3f72c9d3e7b2c26887f6dd1b89431b872e8c6a2c329ee64c8744edf59', '0000000000000000000201fec7d610dd93645f677dfe68afdd1a3b534194e6af'));
```
