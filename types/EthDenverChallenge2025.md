## Type Name

`EthDenverChallenge2025`

## Description

The `EthDenverChallenge2025` query type allows anyone to self report the result of an event specific in-person grip strength dynometer challenge. It will used at ETH Denver 2025, but could also be considered a conceptual query type for other in-person studies.

## Query Parameters

The query data for each event and challenge should be unique and consistent accross all reports for the same event.
The `EthDenverChallenge2025` query type has one parameter for the specific challenge name:

1. challengeName
    - description: descriptor for the challenge (e.g. "grip_strength_dynometer")
    - value type: `string`

Other challenges will be added as needed.


## Query Data

An instance of a `EthDenverChallenge2025 ` query for the grip-strenth dynometer challenge using Solidity:
```s
queryData = abi.encode("EthDenverChallenge2025"), abi.encode("grip_strength_dynometer"))
```

From [the Tellor.io query builder](https://tellor.io/queryidstation/)
```
Query Descriptor:
{"type":"EthDenverChallenge2025","arg1":"grip-strength-dynometer"}

Query Data (Bytes):
0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000001645746844656e7665724368616c6c656e67653230323500000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000017677269702d737472656e6774682d64796e6f6d65746572000000000000000000

Query ID (Hash):
0xaac4e5f5c928963f173417ed5e8a4b998684069693bda10a22923b828958519f
```

## Response Type

The Response (reported value) should be a list of pre-determined event specific values. The result reports 
can be searched on-chain for retroactively engineering corrilations that will look fun on social media. For the grip-strength challenge, we will use a list of 5 values:

```
1. dataSet
    - description: Participant's data set selection (men's or women's data).
    - value type: `bool`
2. rightHand
    - description: grip strength of the participant's right hand.
    - value type: `uint256`
3. leftHand
    - description: grip strength of the participant's left hand.
    - value type: `uint256`
4. XHandle
    - description: the participant's x.com handle.
    - value type: `string`
5. githubUsername
    - description: the participant's github username or email.
    - value type: `string`
6. hoursOfSleep
    - description: estimated number of hours that the participant 
      slept the night before.
    - value type: `uint256`
```

## Example Response

To self report your grip strength and socials for the booth challenge in solidity:
```
pragma solidity 0.8.3;

contract EncodeReport {
    function encodeQueryData(
        string calldata _eventDescription, 
        string calldata _challengeType
    ) public pure returns (bytes32 _queryId, bytes memory _queryData) {
        _queryData = abi.encode("GripDynoChallenge", abi.encode(_eventDescription, _challengeType));
        _queryId = keccak256(_queryData);
    }

    function encodeReportValue(
        int256 _dataSet, 
        uint256 _rightHand,
        uint256 _leftHand,
        string calldata _xHandle,
        string calldata _githubUsername,
        uint256 _hoursOfSleep
    ) public pure returns (bytes memory _value) {
        _value = abi.encode(_dataSet, _rightHand, _leftHand, _xHandle, _githubUsername, _hoursOfSleep);
    }
}
```
If a user input the following values:
`(True, 109, 114.52, 0xspuddy_x, 0xspuddy_git, 4)
```s
_dataSet = "True";
_rightHand = 109000000000000000000;
_leftHand = 114520000000000000000;
_XHandle = "0xspuddy_x"
_githubUsername = "0xspuddy_git"
_hoursOfSleep = 4;
```

The encoded response in bytes:
```
0x0000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000005e8adca7e459400000000000000000000000000000000000000000000000000063548c53f3dbc000000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000a30787370756464795f7800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c30787370756464795f6769740000000000000000000000000000000000000000
```

## Suggested Data Sources

People who visit the Tellor booth at eth denver will self report.

## Dispute Considerations

Since these values are only to be used on testnet, they should only be disputed if it help to make a fun social media post or in person experience.
