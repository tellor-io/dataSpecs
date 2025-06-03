## Query Name

- `Gamut`

## Query Description

This query schema is for use in the gamut system.  It allows a party to make a statement reffering to a given piece of data.  

## Query Parameters

The `Gamut` query has eight parameters. The identifier parameter provides context for the data.


		Argument Schema									
			signer	address	(e.g. eth0x123)				
			parentData	bytes32	(e.g. question 5, )		queryId of parent		
			type	string	(e.g. answer, comment, question , admin, reputation)				
			subType	string	(e.g. new, edit, vote, flag, override, label)				
			category	string	(e.g. fianance)				
			subCategory	string	(e.g. bonds)				
			value	bytes	(e.g. "what is the CPI", upvote)				
			timestamp	uint	(e.g. 11240382)				


1. **signer**
    - description: Party signing the data
    - value type: `address`
2. **parentData**
    - description: queryId of parentData (e.g. question 5)
    - value type: `bytes32`
3. **type**
    - description: Gamut type (e.g. answer, comment, question , admin, reputation)
    - value type: `string`
4. **subType**
    - description: Gamut subtype (e.g. new, edit, vote, flag, override, label)	
    - value type: `string`
5. **category**
    - description: Gamut category (e.g. finance)
    - value type: `string`
6. **subCategory**
    - description: Gamut subCategory (e.g. bonds)
    - value type: `string`
7. **value**
    - description: value of data (e.g. "what is the CPI", upvote)
    - value type: `bytes`
8. **timestamp**
    - description: epoch timestamp (e.g. 11240382)
    - value type: `uint256`

To request new custom price query, please reach out to the Tellor team or make an issue/PR in this repository.

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

## Examples

### STOCKPRICE/NVDA/USD Price

*Query Descriptor:*

```json
{"type":"CustomPrice","identifier":"stockprice","asset":"nvda","currency":"usd","unit":""}
```

*queryData:*

```s
abi.encode("customprice", abi.encode("stockprice","nvda", "usd",""))
```

`00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b637573746f6d70726963650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000006000000000000000000000000000000000000000000000000000000000000000a000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000000a73746f636b70726963650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000046e7664610000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000037573640000000000000000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0x7c55287b5b4172ff494725e55275274558ce159bee6537c1964c42a9a2ba1864`

### LANDX/CORN/USD Price

*Query Descriptor:*

```json
{"type":"CustomPrice","identifier":"landx","asset":"corn","currency":"usd","unit":"megatonne"}
```

*queryData:*

```s
abi.encode("customprice", abi.encode("landx","corn", "usd","megatonne"))
```

`00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000b637573746f6d70726963650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000180000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000014000000000000000000000000000000000000000000000000000000000000000056c616e64780000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000004636f726e000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000003757364000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000096d656761746f6e6e650000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0x8e9773efd4ded8dba1f9d4254d23c4efba00f177f55f2d4eb3673632f32f9524`
     
### Encoding/Decoding

A value of 99.9 would be submitted on-chain using the following bytes:

`0x0000000000000000000000000000000000000000000000056a6418b505860000`

## Dispute Considerations

Reporters should seek guidance from the intended data consumer when selecting data sources and choosing the algorithm to combine them.

- Multiple sources should be used whenever possible.
- When retrieving data directly from and api (exchanges), feed users might expect that exchanges with lower volumes
have their prices weighted accordingly to avoid erratic results.
- Care should also be used when retrieving data from aggregators.
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted.
