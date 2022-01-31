# AWS Spot Price

## Type

`AwsSpotPrice`

## Description

This query asks, "What's the price (USD) per hour to rent a specific [EC2 instance](https://aws.amazon.com/ec2/pricing/on-demand/) in my [region](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html)?"

Why this data? [Morphware](https://morphware.org) needs it. Since Morphware lets people train machine learning models with compute rented from a decentralized network of providers, they require the prices of EC2 instances to help determine the rewards for participating compute providers.

## Parameters

1. `zone`
    - **Description:** a physical location of an AWS data center to rent EC2 instances from.
    - **ABI type:** `string`
    - **Example:** `us-east-1f`
2. `instance`
    - **Description:** AWS virtual server for rent with different CPU, memory, storage, and networking configurations.
    - **ABI type:** `string`
    - **Example:** `i3.16xlarge`

## Expected Response

- **Description:** price per hour in US dollars of an EC2 instance in a specific region. TODO: more
- **ABI type:** `ufixed256x18` (Expected Solidity type to decode the bytes reponse into. More info [here](https://docs.soliditylang.org/en/v0.8.11/types.html#fixed-point-numbers))
- **Packed:** `false` TODO explain
- **Examples:** TODO

    - <details><summary>Python</summary>

        Using [telliot-core](https://github.com/tellor-io/telliot-core):

        ```python
        from telliot_core.api import AwsSpotPrice

        price = 2.345

        query = AwsSpotPrice(zone="us-east-1f", instance="i3.16xlarge")

        value = query.value_type.encode(price)
        print("Encoded query response:", value.hex())
        ```

        Lower level way: TODO

        ```python


        print("Response bytes:", query_data)
        ```

    - <details><summary>JavaScript</summary>

        ```javascript
        console.log("TODO")
        ```

    - <details><summary>Solidity</summary>

        ```javascript
        TODO
        ```

    </details>
    </details>
    </details>

## Query Data
- **Description:** query data is needed to generate the query ID. Query data consists of the bytes encoded query parameter values and query type string. `zone` and `instance` values are encoded first as bytes, then the query type string with those bytes. Order of encoding matters.
- **ABI type:** `bytes`
- **Examples:** See the [catalog](https://github.com/tellor-io/dataSpecs/blob/main/catalog.md) for actively-used query data.

    - <details><summary>Python</summary>

        Using [telliot-core](https://github.com/tellor-io/telliot-core):

        ```python
        from telliot_core.api import AwsSpotPrice

        query = AwsSpotPrice(zone="us-east-1f", instance="i3.16xlarge")

        print("Query Data:", query.query_data)
        ```

        Using [eth-abi](https://github.com/ethereum/eth-abi):

        ```python
        from eth_abi import encode_abi

        encoded_param_vals = encode_abi(["string", "string"], ["us-east-1f", "i3.16xlarge"])

        query_data = encode_abi(["string", "bytes"], ["AwsSpotPrice", encoded_param_vals])

        print("Query Data:", query_data)
        ```

    - <details><summary>JavaScript</summary>

        ```javascript
        console.log("TODO")
        ```

    - <details><summary>Solidity</summary>

        ```javascript
        queryData = abi.encode(["string", "bytes"], ["AwsSpotPrice", encodedParameterValues])
        ```

    </details>
    </details>
    </details>

## Query ID
- **Description:** the unique identifier for this query is constructed by taking the [Keccak hash](https://eth.wiki/en/concepts/ethash/ethash) of the query data bytes. The query ID is used in several Tellor oracle functions, notably `submitValue()`, which is used for reporting query response data.
- **ABI type:** `bytes32`
- **Examples:** See the [catalog](https://github.com/tellor-io/dataSpecs/blob/main/catalog.md) for actively-used query IDs.

    - <details><summary>Python</summary>

        Using [telliot-core](https://github.com/tellor-io/telliot-core):

        ```python
        from telliot_core.api import AwsSpotPrice

        query = AwsSpotPrice(zone="us-east-1f", instance="i3.16xlarge")

        print("Query ID:", query.query_id.hex())
        ```

        Using [web3](https://github.com/ethereum/web3.py):

        ```python
        from web3 import Web3

        query_id = bytes(Web3.keccak(b"fake query data"))

        print("Query ID:", query_id.hex())
        ```

    - <details><summary>JavaScript</summary>

        ```javascript
        console.log("TODO")
        ```

    - <details><summary>Solidity</summary>

        ```javascript
        queryData = abi.encode(["string", "bytes"], ["AwsSpotPrice", encodedParameterValues])
        ```

    </details>
    </details>
    </details>

## JSON Representation

The JSON representation of this query type can be used in a variety of languages to construct query objects.

```json
{
    "type": "AwsSpotPrice",
    "abi": [
        {
            "type": "string",
            "name": "zone",
        },
        {
            "type": "string",
            "name": "instance",
        },
    ],
    "response": {
        "type": "ufixed256x18",
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