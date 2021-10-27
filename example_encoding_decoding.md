# Encoding and decoding guide for `tipQuery` and `submitValue` functions in the TellorX Oracle

This guide explains how to encode and decode parameters of `tipQuery` and `submitValue` function calls in the TellorX Oracle [contract](https://github.com/tellor-io/tellorX/blob/main/contracts/Oracle.sol).

## Encoding example for `submitValue`

Say we want to submit a value to Tellor. Specifically, the price of ETH in USD (ETH/USD). We need to encode a query ID and our value (ETH/USD) in bytes32 and bytes, respectively. From `Oracle.sol`:

```
    /**
     * @dev Allows a reporter to submit a value to the oracle
     * @param _queryId is ID of the specific data feed
     * @param _value is the value the user submits to the oracle
     */
    function submitValue(
        bytes32 _queryId,
        bytes calldata _value,
        uint256 _nonce,
        bytes memory _queryData
    ) external {
        ...
```

First, the query ID. ETH/USD has a corresponding unique identifier (request ID) that existed for legacy Tellor versions before TellorX, so we'll use that to construct the query ID. All legacy IDs (request IDs) are integers from one to 100, ETH/USD being `2`. So the hexadecimal string of the 32 bytes representation of `2` is `0x0000000000000000000000000000000000000000000000000000000000000001`.

Say the current price of ETH is `$4000.05`.