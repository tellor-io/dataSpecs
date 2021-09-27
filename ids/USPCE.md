---
Title: USPCE feed
Author: Brenda Loya
Created: 2021-09-27
---
# Data Spec ID: 7
# Request ID: 

0x0000000000000000000000000000000000000000000000000000000000000029

This ID correlates with a bytes32 version of the old uint256 request ID for the USPCE feed (41).


# Data Specifications

Currently it is is the 3 month rolling average of the USPCE for AMPL as specified here: https://docs.google.com/document/d/1RFCApk1PznMhSRVhiyFl_vBDPA4mP2n1dTmfqjvuTNw/edit


## Description

This is the price target feed for the Ampleforth protocol. It corresponds to their needs and is subject to change with voting.  Currently, it is a three month rolling average of the USPCE, a number which can be found on various government websited.  The data is only updated monthly and is usually manually entered. 


## Granularity

1000000 (6 decimals)

## Example Value (w/ date)

9/27/21 - 115339700


# Dispute Considerations

This is data for the Ampleforth protocol and is subject to change its requirements.  Data from the US government is subject to censorship, reporting delays, and errors.  Be careful when reading thier website and posting information on-chain for use in the protocol. 


# Notes

-
