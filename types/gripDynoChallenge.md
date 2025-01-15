## Type Name

`GripDynoChallenge`

## Description

The `GripDynoChallenge` query type allows anyone to self report the result of an event specific in-person grip strength dynometer challenge. It will used at ETH Denver 2025, but could also be considered a conceptual query type for other in-person studies.

## Query Parameters

The query data for each event or challenge should be unique and consistent accross all reports for the same event.
The `GripDynoChallenge` query type's parameters are defined using a description of the booth event, and the type of challenge:

1. eventDescription
    - description: a clearly written definition of the in-perso event (e.g. "eth_denver_2025")
    - value type: `string`
2. challengeType
    - description: descriptor for the challenge (e.g. "grip_strength_dynometer")
    - value type: `string`

## Response Type

The Response (reported value) should be a list of pre-determined event specific values. These results reports 
can be searched retroactively for engineering corrilations that will look fun on social media. For the grip-strength challenge, we will use a list of 5 values:

```
1. dataSet
    - description: Participant's data set selection (men's or women's data).
    - value type: `int`
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

## Query Data

An instance of a `GripDynoChallenge ` query for the grip-strenth dynometer challenge using Solidity:
```s
queryData = abi.encode("GripDynoChallenge"), abi.encode(query_type, "eth_denver_2025","grip_strength_challenge"))
```
Encoded queryData:
`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000114772697044796e6f4368616c6c656e676500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000c000000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000f6574685f64656e7665725f3230323500000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000017677269705f737472656e6774685f64796e6f6d65746572000000000000000000`

## Query ID

The Query ID is your new Query's unique identifier. It's important to have one because many kinds of data pass through the Tellor ecosystem.

To generate a query ID, get the `bytes32` value of the `keccak` hash of the query data (defined above). For example, in Solidity:
```s
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://tellor.io/queryidstation/) to generate query IDs.


## Example Response

To self report your grip strength and socials for the booth challenge:

```s
uint256 memory _dataSet = "0";
uint256 memory _rightHand = 123;
uint256 memory _leftHand = 34;
string memory _XHandle = "0xSpuddy"
string memory _githubUsername = "0xSpuddy"
string memory _hoursOfSleep = 4;
```

this example response in bytes is...
```
0x0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000007b000000000000000000000000000000000000000000000000000000000000002200000000000000000000000000000000000000000000000000000000000000c0000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000000030000000000000000000000000000000000000000000000000000000000000008307853707564647900000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000083078537075646479000000000000000000000000000000000000000000000000
```

## Suggested Data Sources

People who visit the Tellor booth at eth denver will self report.

## Dispute Considerations

Since these values are only to be used on testnet, they should only be disputed if it help to make a fun social media post or in person experience.