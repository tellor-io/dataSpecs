# Tellor Data Specifications


This repository contains the query specifications for interacting with the TellorX oracle.

A Query specifies how to pose a question to the TellorX oracle, instructions for reporters on how to respond, including the format of the response, and any special dispute considerations.

The following data series are currently supported:

- [Query Catalog](catalog.md)

Check out our queryBuilder tool:  [Query Builder](https://queryidbuilder.herokuapp.com/)


## TLDR

To request a new data feed, make an issue.  

Here's a sample of a price feed one: https://github.com/tellor-io/dataSpecs/issues/24 
Here's a sample of a more custom feed (a non-price feed): https://github.com/tellor-io/dataSpecs/issues/25

That should be all that's necessary, but we'll reach out in the issue with any questions. 


# Query Types

TellorX is designed to support arbitrary query types (`QueryType`).  A `QueryType` can have an arbitrary response type, specified by a structured ABI type string.  

All supported queries are found in the `/types` folder


# Requirements for QueryType Specifications

Every `QueryType` Specification shall provide the following:

## Type

The unique name of the QueryType (e.g. `SpotPrice`)
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
- The parameter order required for the query 

## Response Type

Please specify the ABI type as a valid ETH ABI grammar string(e.g uint256, bytes32, bytes, etc) 

## Example

A working example mapping of all the various inputs and parameters to a valid queryID. 

## Dispute Considerations

Note that following this guide does not prevent you from being disputed or guarantee reporters will properly put a value on-chain. Tellor is decentralized.  This repo is a start to the education necessary for a fully decentralized oracle, but please focus on communication and working with reporters to prevent unneccesary disputes and at the same time encourage monitoring and punishment of bad data. 

# Query Descriptors

Queries are uniquely identified by a special ABI string called the Query `Descriptor`.  In order to properly interact with the oracle, the `Descriptor` string must be provided *exactly* as defined by the [Query Descriptor Specification](#query-descriptor-specifications).

The following is an example descriptor for a legacy query.  The `type` of query is a `LegacyRequest`.  The `LegacyRequest` query has a single integer parameter, called `legacy_id`.

    {"type":"LegacyRequest","legacy_id":1}

All query descriptors should be found in the dataSpecs.json file and shall comply with the following specifications:

1. The top level descriptor string shall be a JSON `object` that defines a set of `type` => `inputs` 

2.  The `type` should be a valid `QueryType`

3. The remaining `inputs` shall be query `parameters`, provided in the specified order.


# QueryData

The `bytes` value of `_queryData` in contract calls shall be the UTF-8 encoded version of the parameters of the query `descriptor` string.


# QueryID

The `bytes32` value of `_queryID` in contract calls shall be the `keccak` hash of `_queryData`.


# Adding New Data Feeds / Types

Make an issue in this repository


## New Data - Existing Query Type

Here's a sample of a price feed one: https://github.com/tellor-io/dataSpecs/issues/24 


## New Data - New QueryType

Here's a sample of a more custom feed (a non-price feed): https://github.com/tellor-io/dataSpecs/issues/25

# Maintainers <a name="maintainers"> </a> 
This repository is maintained by the [Tellor team](https://github.com/orgs/tellor-io/people)


# How to Contribute<a name="how2contribute"> </a>  
Join our Discord or Telegram:
[<img src="./public/telegram.png" width="24" height="24">](https://t.me/tellor)
[<img src="./public/discord.png" width="24" height="24">](https://discord.gg/g99vE5Hb)

Check out our issues log here on Github or feel free to reach out anytime [info@tellor.io](mailto:info@tellor.io).


# Copyright

Tellor Inc. 2022
