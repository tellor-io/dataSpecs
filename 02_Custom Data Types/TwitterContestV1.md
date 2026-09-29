## Name

- `TwitterContestV1`

## Description

Responses to the TwitterContestV1 query type are for the [twitter contest v1](https://github.com/tkernell/twitter-contest) smart contract to determine if a contestant has still maintained their tweeting streak given the contest's parameters (contest start, end, & submission window length, etc.).

## Query Parameters

"For a query with no parameters, one should always include a single empty bytes argument with the name `phantom` as a convention:"

```
1. phantom
   - description: Empty bytes, always used for query types with no arguments
   - value type: `bytes`
```

## Response Type

The response to this query type is a twitter handle of a contestant who has broken their streak. That is, the response is a string of the twitter handle of a contestant who has not tweeted in any elapsed submission window during the current contest.

- `abi_type`: string
- `packed`: false

## Query Data

```solidity
queryData = abi.encode("TwitterContestV1", abi.encode(""))
```
`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000001054776974746572436f6e74657374563100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000000`

## Query ID

```solidity
keccak256(queryData)
```
`0x7848658e5249a88a2cd61c63cb7114555cc79c9a491a7d2edf3b203236917a21`

## Example

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity 0.8.12;
import "usingtellor/contracts/UsingTellor.sol";


contract Example is UsingTellor {

    constructor(address payable _tellorAddress) UsingTellor (_tellorAddress) public {}

    function getLoser(uint256 _index) external view returns (string) {
        bytes memory _queryData = abi.encode("TwitterContestV1", abi.encode(bytes("")));
        bytes32 _queryId = keccak256(_queryData);
        uint256 _timestampRetrieved = getTimestampbyQueryIdandIndex(_queryId, _index);
        require(_timestampRetrieved > 0, "No losers reported to Oracle");
        require(_timestampRetrieved > block.timestamp - 12 hours, "Oracle dispute period has not passed");
        bytes memory _handleBytes = retrieveData(_queryId, _timestampRetrieved);
        return abi.decode(_handleBytes, (string));
    }
}
```

## JSON Representation

```json
{
    "type": "TwitterContestV1",
    "abi": [
        {
            "type": "bytes",
            "name": "phantom",
        },
    ],
    "response": {
        "type": "string",
        "packed": false,
    }
}
```
