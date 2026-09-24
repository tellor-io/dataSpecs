## Query Name

- `ExampleFantasyFootball`

## Query Description

This query is ideal for a hackathon/demo/mvp project. It's for defining some example sports data that'll be reported/retrieved to and from the Tellor Protocol. Specifically, the data contains some player IDs, game IDs, and a fantasy sports rating number. Numbers are used to map to player names (strings) and more information about when the game took place, because this saves gas. Mappings could be stored off-chain. Your choice.

You're putting this data on-chain so reporters can verify the fantasy sports rating number based on the player ID and game ID.

Keep in mind, this is just one way to put your data on chain using Tellor. You can format it however you want! You just need to keep track of the unique identifier for your specific data (queryId), so you can distinguish it from all the other types of data pushed to Tellor. And ideally, you help develop a specification for you desired data, so others can reproduce it and report it on-chain for you.

## Query Parameters

The `ExampleFantasyFootball` query has one parameter:

1. **version**:
    - description: Version number, in case you make more versions of this data specification
    - value type: `int256`

## Response Type

The query response will consist of an array of integers. The array includes a player ID, followed by a game ID, followed by fantasy sports rating... For example:

Given the two mappings which you have defined on-chain or off-chain somewhere:
```python
player_ids_to_names = {
    20889:"Kyler Murray",
    18890:"Patrick Mahomes",
    16762:"Jameis Winston",
    17922:"Jared Goff",
}
game_ids_to_game_info = {
    1234:"week 1", # plus maybe more info
    5678:"week 2",
    2222:"week 3",
}
```

We'll also need to convert the decimal, or `float`, value of the fantasy sports rating for that player to an integer, since Solidity doesn't use floats. Given a rating of 20.55 and a decimal precision of 18 places, you would report `20550000000000000000`.

So if you wanted to report that fantasy rating for each player, the response to this query, or the value before being encoded to bytes would be:
```python
data = [20889,2222, 20550000000000000000, 18890,2222, 20550000000000000000,16762,2222, 20550000000000000000]
```
`data` contains information for three players. Player IDs for Kyler Murray, Patrick Mahomes, and Jameis Winston are each followed by a game ID, which all happen to be `2222` (week 3).

Solidity type:
- `abi_type`: `int256[]`
- `packed`: false

An example of encoding and decoding this response type using Python:

```python
from eth_abi import encode_abi, decode_abi

data = [20889, 2222, int(20.55*10**18), 18890, 2222, int(20.55*10**18),16762, 2222, int(20.55*10**18)]
submit_value = encode_abi(["int256[]"], [data])
print(submit_value.hex())
# 00000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000009000000000000000000000000000000000000000000000000000000000000519900000000000000000000000000000000000000000000000000000000000008ae0000000000000000000000000000000000000000000000011d30441f1647000000000000000000000000000000000000000000000000000000000000000049ca00000000000000000000000000000000000000000000000000000000000008ae0000000000000000000000000000000000000000000000011d30441f16470000000000000000000000000000000000000000000000000000000000000000417a00000000000000000000000000000000000000000000000000000000000008ae0000000000000000000000000000000000000000000000011d30441f16470000

decoded_val = decode_abi(["int256[]"], submit_value)
print(decoded_val)
# ((20889, 2222, 20550000000000000000, 18890, 2222, 20550000000000000000, 16762, 2222, 20550000000000000000),)
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

## Decoding submitted value in solidity
More Solidity help [here](https://tellor.io/docs/).

```js
pragma solidity >=0.8.0;

import "usingtellor/contracts/UsingTellor.sol";

contract MyContract is UsingTellor {

  constructor(address payable _tellorAddress) UsingTellor (_tellorAddress) public {}

  function getFirstPlayerFantasyStat() external view returns(int256) {
    
      bytes memory _queryData = abi.encode("ExampleFantasyFootball", abi.encode(1));
      bytes32 _queryId = keccak256(_queryData);
      
      (bool ifRetrieve, bytes memory _value, ) =
          getDataBefore(_queryId, block.timestamp - 1 hours);
      if (!ifRetrieve) return 0;
      // Returns Kyle Murray's fantasy sports rating for week 3
      return abi.decode(_value, (int256[]))[2];
    }

}
```

An alternative way to structure the data could be to use a struct array which may help with the user's code readability:

```js
SportsData[] public sportsArray;

struct SportsData {
    int256 playerId;
    int256 weekId;
    int256 rating;
}

function encodeSportsArray() public view returns(bytes memory) {
    return abi.encode(sportsArray);
}

function decodeSportsArray(bytes memory _value) public pure returns(SportsData[] memory) {
    return abi.decode(_value, (SportsData[]));
}

function decodeSportsArrayAndGetRating(bytes memory _value, uint256 _index) public pure returns(int256) {
     // returns rating for player at _index
     return abi.decode(_value, (SportsData[]))[_index].rating;
}
```
Depending on the implementation, you could even remove the playerId from the reported data if the user's smart contract contained a record of each player's expected position in the reported array.
