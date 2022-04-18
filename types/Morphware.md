# Morphware

## Type

`Morphware`

## Description

Cloud compute prices and metadata. [Morphware](https://morphware.org) needs rental prices of cloud compute providers to help determine the rewards for participating compute providers in their decentralized network.

## Parameters

1. `version`
   - **Description:** Morphware query version. Morphware provides detailed specs for each version [here](https://github.com/morphware/daemon/blob/TellorOracleDocs/docs/TellorSpotPriceOracle.md).
   - **ABI type:** `uint256`
   - **Example:** `1`

## Expected Response

- **Description:** An array of JSON strings containing cloud compute prices and metadata. Refer to the detailed response requirements of each Morphware query version [here](https://github.com/morphware/daemon/blob/TellorOracleDocs/docs/TellorSpotPriceOracle.md).
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
      "name": "version"
    }
  ],
  "response": {
    "type": "string[]",
    "packed": false
  }
}
```

## Dispute Considerations & Potential Data Sources

Reporters should review the dispute considerations and suggested data sources for each Morphware query version [here](https://github.com/morphware/daemon/blob/TellorOracleDocs/docs/TellorSpotPriceOracle.md).
