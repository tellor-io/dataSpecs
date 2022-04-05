This is an example template for a new Query type. Every `Query` specification must include the following:


## Type Name

A unique name of your new Query type (e.g. `SpotPrice`, `Snapshot`). The markdown file you create based off this template will be named using the query's type name:

    <query-type-name>.md


## Description

A general description of the Query type, including its purpose and suggested use cases.


## Query Parameters

A query's parameters may change for each instance of your query type. For example, a `SpotPrice` query has both an `asset` and `currency` parameter. One instance of a `SpotPrice` query could have parameter values of `eth` (asset) and `usd` (currency), while another's `currency` parameter value might be `eur`. The definition of your new query type's parameters must each include:
  - Parameter name (a string)
  - Description and value specification
  - Valid ABI type
  - The parameter order required for the query data bytes (more on this later)

For example, the `SpotPrice` query type's parameters are defines as:
```
1. asset
    - description: Asset ID (e.g. BTC)
    - value type: `string`
2. currency
    - description: Selected currency (e.g. USD)
    - value type: `string`
```


## Response Type

Specify the ABI type as a valid ETH ABI grammar string(e.g uint256, bytes32, bytes, etc), as well as a boolean signifying whether the reponse will be packed or not when encoded to bytes. More on valid types [here](https://docs.soliditylang.org/en/v0.8.13/types.html).

For example, the `SpotPrice`'s response type an unpacked 256 bit value with 18 decimals of precision:
```
- abi_type: ufixed256x18 (18 decimals of precision)
- packed: false
```


## JSON Representation
The JSON representation of this query type is needed to construct query objects in a variety of languages.

For example, the JSON representation of a `SpotPrice` query:
```json
{
    "type": "SpotPrice",
    "abi": [
        {
            "type": "string",
            "name": "asset",
        },
        {
            "type": "string",
            "name": "currency",
        },
    ],
    "response": {
        "type": "ufixed256x18",
        "packed": false,
    }
}
```


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