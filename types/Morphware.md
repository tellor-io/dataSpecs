# Morphware AWS Instance Spot Price Feed

## Query Name

- `MorphwareAWSSpotPriceQuery`

## Query Description

[Morphware](https://morphware.org) lets people train machine learning models with compute rented from a decentralized network of providers. Their service needs the prices of [EC2 instances on AWS](https://aws.amazon.com/ec2/pricing/on-demand/) in US dollars to help determine the rewards for participating compute providers.

## Query Parameters

The `MorphwareAWSSpotPriceQuery` query has two parameters:

**Zone**: physical locations of AWS data center clusters (example: us-east-1f)

**Instance**: compute optimized, memory optimized, etc. (example: i3.16xlarge)

## Query Response

**Price**: price per hour in USD (example: 2.1194)

*Query Descriptor:*

(json)
```json
{
    "name": "MorphwareAWSSpotPriceQuery",
    "type": "function", 
    "inputs": [
        {
            "type": "string",
            "name": "zone",
        },
        {
            "type": "string",
            "name": "instance",
        },
        {
            "type": "int",
            "name": "price",
        }
    ],
}
```

## Encoding Response

If you'd like to report a value for this query, you'll need to format and encode your query response for use as the bytes `value` argument of the oracle's `submitValue` function. 

For this query type, we'll concatinate our response as a string, using the query parameter names as separators, then utf-8 encode that string to bytes. We can't simply encode a json object, because Solidity doesn't play nice with special characters (#TODO Tim, please provide the correct reason if I'm wrong).

Examples:

### Python

Using [telliot](https://github.com/tellor-io/telliot-core):
```python
from telliot_core.api import MorphwareAWSSpotPriceQuery

zone = "us-east-1f"
instance_type = "i3.16xlarge"
price = "2.1194"

formatted_query_response = f"zone{zone}instance{instance_type}price{price}"

print("Formatted query response:", formatted_query_response)

query = MorphwareAWSSpotPriceQuery()

encoded_value = query.value_type.encode(value)
print("Encoded query response:", encoded_value.hex())
```
Output:
```
0x00001234thisisfakeboi1234
```

## Decoding Response

If you'd like to fetch and parse the latest reported response to this query... #TODO

## Dispute Considerations

Reporters should use care in selecting data sources and choosing the algorithm to combine them.
