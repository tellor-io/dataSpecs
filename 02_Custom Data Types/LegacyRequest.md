## Query Name

- `LegacyRequest`

## Query Description

This is the type for all legacy requests (created and running before 12/01/2021), you should use the `SpotPrice` query type instead.
## Query Parameters

The `LegacyRequest` query has one parameter, which specifies the requested data.  

1. **requestId** (uint256): unique identifier of the request. Each `requestId` maps to a an asset/currency pair. For example `requestId` 1 means the spot price of ETH in USD.

The `requestId` should be a requestId from the Tellor system before the TellorX upgrade. Unlike all newer `queryId`s, the hash of the `queryData` is not the `queryId` for `LegacyRequest` Query types. 


## Granularity

1000000 (6 decimals)

## Example Value (w/ date)

9/27/21 - 3000000000


## Supported Data

Only certain ID's are valid for legacy requests.  The others will be considered null and any price feeds wanted other than this list should use the `SpotPrice` type.

Existing LegacyRequest queries:
    - `requestId`: 1 (ETH/USD)
    - `requestId`: 2 (BTC/USD)
    - `requestId`: 10 (AMPL/USD)
    - `requestId`: 41 (USPCE value)
    - `requestId`: 50 (TRB/USD)
    - `requestId`: 59 (ETH/JPY)

# Dispute Considerations

Be sure to use multiple exchanges, as one exchange could potentially slip out of alignment with others.  Also, be sure to note w/ regard to the "current" specification, that Ethereum blocks can take time to mine and prices move quickly.  A valid answer may not correspond to reality when it gets accepted in a block. 

# Notes

-

