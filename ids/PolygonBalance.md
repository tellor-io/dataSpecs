---
Title: PolygonBalance.md
Author: Brenda Loya
Created: 2021-09-13
---

# Request ID: 

0x0000000000000000000000000000000000000000000000000000000000000001

This ID correlates with a bytes32 version of the old uint256 request ID for the ETH/USD feed.


# Data Specifications

## Description

This is the feed for the ETH/USD price. It represents a valid ETH/USD price at the time the data is placed on-chain.  


## Granularity

1000000 (6 decimals)

## Example Value (w/ date)

3214870000 - 9/13/21


## Dispute Considerations

Crypto-currency prices can be volatile, with large deviations even amongst various exchanges.  This feed is a broad ETH/USD feed and does not represent the price at any one specific exchange or at any point in the day.  Therefore, the range of acceptable values is likely larger than any one specific API feed. 


# Notes


