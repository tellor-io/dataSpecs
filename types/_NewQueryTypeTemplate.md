Tellor is designed to support arbitrary query types (`QueryType`).  A `QueryType` can have an arbitrary response type, specified by a structured ABI type string.  

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

## Query Descriptors

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


Reporters should use care in selecting data sources and choosing the algorithm to combine them.
 
- Multiple sources should be used whenever possible.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted.

## Data Sources

AWS pricing can be retrieved through a number of APIs like [Amazon's](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/price-changes.html) or [infracost](https://www.infracost.io/blog/cloud-pricing-api/). Also, there are tools like [cloudinfo](https://github.com/banzaicloud/cloudinfo) for retrieving responses to this query.