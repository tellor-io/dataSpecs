# Tellor Data Specifications


This repository contains the query specifications for interacting with the TellorX oracle.

A Query specifies how to pose a question to the TellorX oracle, instructions for reporters on how to respond, including the format of the response, and any special dispute considerations.


Check out our queryBuilder tool:  [Query Builder](https://queryidbuilder.herokuapp.com/)


## TLDR

A "QueryType" is the unique name of the query. All QueryTypes should have at least one parameter being "type", which is mapped to a .md file in this dataSpecs repo.  

The "Query Descriptor" is a structured ABI string with details of each input variable

"QueryData" is the input to the _queryData field in the submitValue function on Tellor and represents the encoded parameters of the QueryType (abi.encode() in soldity)

A "QueryID" is the hash (keccack256) of the QueryData


# Query Types

TellorX is designed to support arbitrary query types (`QueryType`).  A `QueryType` can have an arbitrary response type, specified by a structured ABI type string.  

The following `QueryType`s are currently supported:

- [Pricing Queries](price/README.md)
- [Legacy Queries](legacy/README.md)
- [Project Specific](projectSpecific/README.md)

A `QueryType` can have multiple Query `parameters` that specify details of the query request (e.g. the token symbol for a `CoinPrice` query).

# Query Descriptors

Queries are uniquely identified by a special ABI string called the Query `Descriptor`.  In order to properly interact with the oracle, the `Descriptor` string must be provided *exactly* as defined by the [Query Descriptor Specification](#query-descriptor-specifications).

The following is an example descriptor for a legacy query.  The `type` of query is a `LegacyRequest`.  The `LegacyRequest` query has a single integer parameter, called `legacy_id`.

    {"type":"LegacyRequest","legacy_id":1}

For further details, refer to [Query Descriptor Specifications](#query-descriptor-specifications) and the `QueryType` specifications for the query of interest.

# Requirements for QueryType Specifications

Every `QueryType` Specification shall provide the following:

## Type

The unique name of the QueryType (e.g. `LegacyRequest`)
Further details of the `QueryType` specification shall be provided in a file named: 

    <query-type-name>.md

## Description

A general description of the QueryType, including its purpose and suggested use cases.

## Query Parameters

A description of the query parameters shall include:

- A table defining the following for each `parameter`:
  - `Type`
  - `Description`
  - `Data Type`
  - `Value Specifications`
- The parameter order required by the query `Descriptor`
- Valid Parameter Sets
  - A list of valid parameter sets that are expected to be supported by reporters
- Active Parameter Sets (optional)
  - A list of parameter sets that are *actively* supported by current reporters


## Response Type

The response type shall specify two values
- `abi_type`
- `packed`

The ABI type shall be specified as a valid ETH ABI grammar string. Examples include:

    uint256
    fixed256x9
    bytes32
    bytes64
    (int8,bytes,ufixed32x9,bool)[2]

The response type shall also include whether the data shall be ABI encoded in packed (`packed=True`) or (`packed=False`) unpacked format. We currently recommend using unpacked formats only.

The response type for all LegacyRequest queries is a 256-bit unsigned integer, with 6 decimals of precision, i.e.:

    abi_type="ufixed256x6"
    packed=False


## Example

An example demonstrating detailed usage.

## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guaruntee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 


# Query Descriptor Specifications
All query descriptors should be found in the dataSpecs.json file and shall comply with the following specifications:

1. The top level descriptor string shall be a JSON `object` that defines a set of `type` => `inputs` 

2.  The `type` should be a valid `QueryType`

3. The remaining `inputs` shall be query `parameters`, provided in the specified order.

4. There shall be no optional query parameters.  All query parameters shall be specified in the descriptor.

## Contract Interfaces

Interacting with the TellorX contract (e.g. calls to  `tipQuery`, `submitValue`) requires the computation of two values: `_queryData`, and `_queryID`.  

Both values are easily computed from the QueryType Descriptor JSON string.
 

### QueryData

The `bytes` value of `_queryData` in contract calls shall be the UTF-8 encoded version of the parameters of the query `descriptor` string.


### QueryID

The `bytes32` value of `_queryID` in contract calls shall be the `keccak` hash of `_queryData`, with a single exception for the `LegacyRequest` type.

The `LegacyRequest` provides support for users of the Tellor network prior to TellorX.  In this case, the `_queryID` shall be set to the `request_id` defined in [Legacy Data Feed IDs](https://docs.tellor.io/tellor/integration/data-ids).
 

# Adding New Query Types

Make a PR in this repo to define the new `QueryType` specification following the 
[Requirements for QueryType Specifications](#requirements-for-querytype-specifications).

# Maintainers <a name="maintainers"> </a> 
This repository is maintained by the [Tellor team](https://github.com/orgs/tellor-io/people)


# How to Contribute<a name="how2contribute"> </a>  
Join our Discord or Telegram:
[<img src="./public/telegram.png" width="24" height="24">](https://t.me/tellor)
[<img src="./public/discord.png" width="24" height="24">](https://discord.gg/g99vE5Hb)

Check out our issues log here on Github or feel free to reach out anytime [info@tellor.io](mailto:info@tellor.io).


# Disclaimer  

This is just a guide.  Tellor is decentralized, as with the voting, so nothing is promised.  Mine or use the data at your own risk.

Any API examples are only examples and not an endorsement.  It is the reporter's job to find valid data sources.

Work in a collaborative manner on discord.  If a value is a little stale or a slightly off, be sure to just ping reminders to update or get it off.  It's hard to say what is or is not a good value unless it's painfully obvious, so as the system continues to work out the tweaks, please keep a wide margin of error before starting disputes.

It's hard to say "if 10% off" or any hard line value.  Sometimes ETH moves 10% in 5 minutes, so both could be valid.  You really have to use judgement, and even more so you have to realize that disputes are handled by a community consensus, not some formula.


# Copyright

Tellor Inc. 2022
