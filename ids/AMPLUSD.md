---
Title: AMPL/USD feed
Author: Brenda Loya
Created: 2021-09-09
---
# Data Spec ID: 1

# Request ID: 

0x000000000000000000000000000000000000000000000000000000000000000a

This ID correlates with a bytes32 version of the old uint256 request ID 10 for the AMPL/USD feed.


# Data Specifications

This is the feed for AMPL and specifications are mantained by the AMPL team here: https://docs.google.com/document/d/1RFCApk1PznMhSRVhiyFl_vBDPA4mP2n1dTmfqjvuTNw/edit


## Description

This is the AMPL/USD feed that corresoponds to the data needed by the AMPL protocol.  It is a flexible ID that does not correlate to one specific price feed or methodology and is subject to change by FORTH governance. Be sure to follow the Ampleforth community to stay up to date with their needs. 

Currently this data represents a 24hr TWAP/VWAP of the AMPL/USD price as specified in their documentation. 


## Granularity

1000000 (6 decimals)

## Example Value (w/ date)

9/27/2021 - 928295


# Dispute Considerations

Disputes should correspond with the needs and desires of the AMPL community.  Do not assume that a valid AMPL/USD price will simply suffice.  They have a very specific calculation that is based on a broad definition. Look at current impletations by the AMPL team and LINK for examples of accepted data. 

# Notes


