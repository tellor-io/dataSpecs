# Type Name: `EthDenver2025`

## Description

The `EthDenver2025` query type allows anyone to self report the result of event in-person challenges. It will used at ETH Denver 2025, but could also be considered a conceptual query type for other in-person studies.

## Query Parameters

The query data for each event and challenge should be unique and consistent accross all reports for the same event.
The `EthDenver2025` query type has one parameter for the specific challenge name:

1. challengeName
    - description: descriptor for the challenge (e.g. "grip_strength_dynamometer")
    - value type: `string`

Other challenges will be added as needed.

## Query Data

An instance of a `EthDenver2025` query for the grip-strenth dynamometer challenge using Solidity:

```s
queryData = abi.encode("EthDenver2025"), abi.encode("grip_strength_dynamometer"))
```

encoded query data:

```s
0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000d45746844656e7665723230323500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000200000000000000000000000000000000000000000000000000000000000000019677269705f737472656e6774685f64796e616d6f6d6574657200000000000000
```

encoded query Id:

```s
0x5d1bd59b60b85541794db87f4b5b2128f583a97800ea6c695378211b29bd1815
```

From [the Tellor.io query builder](https://tellor.io/queryidstation/)

## Response Type

The Response (reported value) should be a list of pre-determined event specific values. The result reports 
can be searched on-chain for retroactively engineering corrilations that will look fun on social media. For the grip-strength challenge, we will use a list of 5 values:

```yaml
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

```s
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

```s
0x0000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000005e8adca7e459400000000000000000000000000000000000000000000000000063548c53f3dbc000000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000001000000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000a30787370756464795f7800000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c30787370756464795f6769740000000000000000000000000000000000000000
```

## Suggested Data Sources

People who visit the Tellor booth at eth denver will self report.

## Dispute Considerations

Since these values are only to be used on testnet, they should only be disputed if it help to make a fun social media post or in person experience.
