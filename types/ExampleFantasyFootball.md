## Query Name

- `ExampleFantasyFootball`

## Query Description

This query is ideal for a hackathon/demo/mvp project. It's for defining some example sports data that'll be reported/retrieved to and from the Tellor Protocol. Specifically, the data contains some player IDs and positions. Numbers are used to map to player names (strings) and position names (strings), because this saves gas. The entire reference mapping of player IDs -> player names and position IDs -> position names could be stored on-chain or off-chain. Your choice.

Keep in mind, this is just one way to put your data on chain using Tellor. You can format it however you want! You just need to keep track of the unique identifier for your specific data (queryId), so you can distinguish it from all the other types of data pushed to Tellor. And ideally, you help develop a specification for you desired data, so others can reproduce it and report it on-chain for you.

## Query Parameters

The `ExampleFantasyFootball` query has two parameters:

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
`data` contains the player IDs for Kyler Murray, Patrick Mahomes, and Jameis Winston, who all happen to be QBs.

Solidity type:
- `abi_type`: `uint256[]`
- `packed`: false

An example of encoding and decoding this response type using Python:

```python
from eth_abi import encode_abi
data = [20889,1,18890,1,16762,1]
submit_value = encode_abi(["uint256[]"], data)
print(submit_value)
```
