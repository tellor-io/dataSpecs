# Tellor Data Specifications

This repository contains the query specifications for interacting with the TellorX oracle.

A Query specifies how to pose a question to the TellorX oracle, instructions for reporters on how to respond, including the format of the response, and any special dispute considerations.

# Query Types

TellorX is designed to support arbitrary query types (`QueryType`).  A `QueryType` can have an arbitrary response type, specified by a structured
ABI type string.  

The following `QueryType`s are currently supported:

- LegacyRequest
- TextQuery
- CoinPrice (Under development)

A `QueryType` can have multiple Query `parameter`s that specify details of the query request (e.g. the token symbol for a `CoinPrice` query).

Please submit a PR to this repository or contact the team if you would like to define a new `QueryType`. 

# Query Descriptors

Queries are uniquely identified by a special JSON string called the Query `Descriptor`.  In order to properly interact with the oracle, the `Descriptor` string must be provided *exactly* as defined by the [Query Descriptor Specification](#query-descriptor-specifications).

The following is an example descriptor for a Legacy Query.  The `type` of query is a `LegacyRequest`.  The `LegacyRequest` query has a single integer parameter, called `legacy_id`.

    {"type":"LegacyRequest","legacy_id":1}

For further details, refer to [Query Descriptor Specifications](#query-descriptor-specifications) and the `QueryType` specifications for
the query of interest.


# Requirements for QueryType Specifications

Every `QueryType` Specification shall provide the following:

## Name

The unique name of the QueryType (e.g. `LegacyRequest`)
Further details of the `QueryType` specification shall be provided in a file named: 

    <query-type-name>.md

## Description

A general description of the QueryType, including its purpose and suggested sue cases.

## Query Parameters

A description of the query parameters shall include:

- A table defining the following for each `parameter`:
  - `Name`
  - `Description`
  - `Data Type`
  - `Value Specifications`
- The parameter order required by the query `Descriptor`.
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

The response type shall also include whether the data shall be ABI encoded in
packed (`packed=True`) or (`packed=False`) unpacked format. 
We currently recommend using unpacked formats only.


## Data Specifications

Technical data specifications


## Example

An example demonstrating detailed usage.

## Dispute Considerations

Considerations that parties should consider before reporting the data w/ regard to what will be considered a valid dispute

## Notes

Any further notes or thoughts on the query type/


# Query Descriptor Specifications
All query descriptors shall comply with the following specifications:

1. The top level descriptor string shall be a JSON `object` that defines a set of `name`/`value` pairs.

2.  An object begins with `{`  and ends with `}`. Each name is followed by `:` colon and the name/value pairs are separated by a comma (`,`).  

3. There shall be **no whitespace before or after the colon or comma separators**.

4. The top level object is ordered so that `name`/`value` pairs must be provided in the specified order.

5. The first `name`/`value` pair shall provide the Query Type
   - The `name` shall be the word `"type"`.  
   - The `value` shall be the name of a valid `QueryType`.

6. The remaining `name`/`value` pairs shall be query `parameters`, provided in the specified order.

7. There shall be no optional query parameters.  All query parameters shall be specified in the descriptor.

## Contract Interfaces

Interacting with the TellorX contract (e.g. calls to  `tipQuery`, `submitValue`) requires the computation of two values: `_queryData`, and `_queryID`.  

Both values are easily computed from the QueryType Descriptor JSON string.


### QueryData

The `bytes` value of `_queryData` in contract calls shall be the UTF-8 encoded version of the query `descriptor` string.


### QueryID

The `bytes32` value of `_queryID` in contract calls shall be the `keccak` hash of `_queryData`, with a single exception for the `LegacyRequest` type.

The `LegacyRequest` provides support for users of the Tellor network prior
to TellorX.  In this case, the `_queryID` shall be set to the `request_id` defined
in [Legacy Data Feed IDs](https://docs.tellor.io/tellor/integration/data-ids).
 

# Adding New Query Types

Make a PR in this repo to define the new `QueryType` specification following the 
[Requirements for QueryType Specifications](#requirements-for-querytype-specifications).

# Maintainers <a name="maintainers"> </a> 
This repository is maintained by the [Tellor team](https://github.com/orgs/tellor-io/people)


# How to Contribute<a name="how2contribute"> </a>  
Join our Discord or Telegram:
[<img src="./public/telegram.png" width="24" height="24">](https://t.me/tellor)
[<img src="./public/discord.png" width="24" height="24">](https://discord.gg/g99vE5Hb)

Check out our issues log here on Github or feel free to reach out anytime [info@tellor.io](mailto:info@tellor.io)


# Disclaimer  

This is just a guide.  Tellor is decentralized, as with the voting, so nothing is promised.  Mine or use the data at your own risk

Any API examples are only examples and not an endorsement.  It is the reporter's job to find valid data sources

Work in a collaborative manner on discord.  If a value is a little stale or a slightly off, be sure to just ping reminders to update or get it off.  It's hard to say what is or is not a good value unless it's painfully obvious, so as the system continues to work out the tweaks, please keep a wide margin of error before starting disputes

It's hard to say "if 10% off" or any hard line value.  Sometimes ETH moves 10% in 5 minutes, so both could be valid.  You really have to use judgement, and even more so you have to realize that disputes are handled by a community consensus, not some formula


# Copyright

Tellor Inc. 2021