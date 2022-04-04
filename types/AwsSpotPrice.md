# Morphware

## Type

`Morphware`

## Description

Cloud compute prices and metadata. [Morphware](https://morphware.org) needs rental prices of cloud compute providers to help determine the rewards for participating compute providers in their decentralized network.

## Parameters

1. `version`
    - **Description:** Morphware query version. Morphware provides detailed specs for each version [here](TODO: get link to Morphware docs).
    - **ABI type:** `uint256`
    - **Example:** `1`


## Expected Response

- **Description:** An array of JSON strings containing cloud compute prices and metadata. Refer to the detailed response requirements of each Morphware query version [here](TODO: link to Morphware docs).
- **ABI type:** `string[]` (Expected Solidity type to decode the bytes reponse into. More info [here](https://docs.soliditylang.org/en/v0.8.11/types.html#fixed-point-numbers))
- **Packed:** `false`
- **Examples:** [here](https://github.com/tellor-io/telliot-core/blob/main/tests/test_query_morphware.py)


## Query Data
- **Description:** Query data is needed to generate the query ID. To generate the query data, the `version` parameter value (e.g. `1`) is first encoded as bytes, then the query type string with those bytes. Order of encoding matters.
- **ABI type:** `bytes`
- **Examples:** [here](https://github.com/tellor-io/telliot-core/blob/main/tests/test_query_morphware.py)

## Query ID
- **Description:** The unique identifier for this query is constructed by taking the [Keccak hash](https://eth.wiki/en/concepts/ethash/ethash) of the query data bytes.
- **ABI type:** `bytes32`
- **Examples:** [here](https://github.com/tellor-io/telliot-core/blob/main/tests/test_query_morphware.py)

## JSON Representation

The JSON representation of this query type can be used in a variety of languages to construct query objects.

```json
{
    "type": "Morphware",
    "abi": [
        {
            "type": "uint256",
            "name": "version",
        },
    ],
    "response": {
        "type": "string[]",
        "packed": false,
    }
}
```

## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.
 
- Multiple sources should be used whenever possible.
- Care should also be used when retrieving data from aggregators.  
- It is the reporters responsibility to ensure that the feed result is *reasonable* enough for a community consensus, otherwise it may be subject to dispute.
- If a *reasonable* value cannot be determined, a value should not be submitted.

## Data Sources

AWS pricing can be retrieved through a number of APIs like [Amazon's](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/price-changes.html) or [infracost](https://www.infracost.io/blog/cloud-pricing-api/). Also, there are tools like [cloudinfo](https://github.com/banzaicloud/cloudinfo) for retrieving responses to this query.