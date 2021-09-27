---
Title: TRB/USD feed
Author: Brenda Loya
Created: 2021-09-27
---
# Data Spec ID: 6

# Request ID: 

0x0000000000000000000000000000000000000000000000000000000000000032

This ID correlates with a bytes32 version of the old uint256 request ID for the TRB/USD feed (50).


# Data Specifications

A current TRB/USD price.


## Description

This is the ID for a valid TRB/USD price.  It does not specify which exchanges, but does not mean a price on any exchange.  Rather, it is the ambiguous current price which represents what parties would agree on as a given price for the current timestamp. 


## Granularity

1000000 (6 decimals)

## Example Value (w/ date)

9/27/21 - 45210000


# Dispute Considerations

Be sure to use multiple exchanges, as one exchange could potentially slip out of alignment with others.  Also, be sure to note w/ regard to the "current" specification, that Ethereum blocks can take time to mine and prices move quickly.  A valid answer may not correspond to reality when it gets accepted in a block. 

# Notes


