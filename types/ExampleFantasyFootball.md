## Query Name

- `ExampleFantasyFootball`

## Query Description

This query is ideal for a hackathon/demo/mvp project. It's for defining some example sports data that'll be reported/retrieved to and from the Tellor Protocol. Specifically, the data contains some player IDs and positions. Numbers are used to map to player names (strings) and position names (strings), because this saves gas. The entire reference mapping of player IDs -> player names and position IDs -> position names could be stored on-chain or off-chain. Your choice.

Keep in mind, this is just one way to put your data on chain using Tellor. You can format it however you want! You just need to keep track of the unique identifier for your specific data (queryId), so you can distinguish it from all the other types of data pushed to Tellor. And ideally, you help develop a specification for you desired data, so others can reproduce it and report it on-chain for you.

## Query Parameters

The `ExampleFantasyFootball` query has one parameter:

1. **version**:
    - description: Version number, in case you make more versions of this data specification
    - value type: `uint256`

## Response Type

The query response will consist of an array of integers. The array includes a player ID followed by a position ID, followed by another player ID... For example:

Given the two mappings which you have defined on-chain or off-chain somewhere:
```python
player_ids_to_names = {
    20889:"Kyler Murray",
    18890:"Patrick Mahomes",
    16762:"Jameis Winston",
    17922:"Jared Goff",
}
player_positions_to_names = {
    1:"QB",
    2:"WR",
    3:"TE",
}
```
And the following info you want to put on chain (the response to this query):
```python
data = [20889,1,18890,1,16762,1]
```
`data` contains information for three players. Player IDs for Kyler Murray, Patrick Mahomes, and Jameis Winston are each followed by a position ID, which all happen to be `1` (QB).

Solidity type:
- `abi_type`: `uint256[]`
- `packed`: false

An example of encoding and decoding this response type using Python:

```python
from eth_abi import encode_abi, decode_abi

data = [20889,1,18890,1,16762,1]
submit_value = encode_abi(["uint256[]"], data)
print(submit_value.hex())
# 0x000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000005199000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000049ca0000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000417a0000000000000000000000000000000000000000000000000000000000000001

decoded_val = decode_abi(["uint256[]"], submit_value)
print(decoded_val)
# ((20889,1,18890,1,16762,1),)
```

## Example queryData and queryId
(Solidity)

_queryData:_

```js
// First encode the query parameter, 1, for version 1
// Then encode those bytes with the query type string, "ExampleFantasyFootball"
abi.encode("ExampleFantasyFootball", abi.encode(1))
```

`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000164578616d706c6546616e74617379466f6f7462616c6c0000000000000000000000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000001`

_queryID:_

```js
// Generate the unique identifier for your data, this query instance, by taking the keccak hash of the query data (generated above)
keccak256(abi.encode("ExampleFantasyFootball", abi.encode(1)))
```

`0xe1defa9341b970361595a2019126e8b028e2f9bc5ba298f2228d33bd4d5fa86e`
