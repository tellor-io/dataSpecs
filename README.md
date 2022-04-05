# Tellor Data Specifications


This repository contains specifications of data requested from, reported to, and retrieved from Tellor oracles. These specifications are called Queries.

- [For Tellor Users](#for-tellor-users)
- [For Tellor Reporters](#for-tellor-reporters)
- [Create new Query type](#create-new-query-type)

## For Tellor Users:

### **Price data**:
First check if the price data is already being reported [here]().

Need a spot price from Tellor? Generate a unique identifier, or query ID, for the `asset/currency` pair using [this tool](https://queryidbuilder.herokuapp.com/). After, use that ID to [pay reporters](https://github.com/tellor-io/autoPay) to put that data on-chain. Then retrieve the reported spot price from from the oracle [like this](https://docs.tellor.io/tellor/integration/introduction).

### **Custom data**:
First, check in [`/types`](./types/) if there's already a query type that defines the data you need.

If not, [create a new query type](#create-new-query-type) and [pay reporters](https://github.com/tellor-io/autoPay) to put that data on-chain, and retrieve that data from a Tellor oracle [like this](https://docs.tellor.io/tellor/integration/introduction).

## For Tellor Reporters:
To find out what data to report, there's two options:
- Use our [reporter client]() to automatically report the expected response of queries that Tellor users are funding.
- Check for newly funded queries, then refer to that query's expected response in [`/types`](./types/) for the required data to report. Be sure to read the dispute considerations section for that query.


## Create New Query Type
To create a new Query, or specification, for custom data you need from Tellor oracles, there's two options:
- Make an issue like [this](https://github.com/tellor-io/dataSpecs/issues/25) and the Tellor team will help with the next steps.
- Fork this repository and make a pull request for a new Query type in `./types` using [this template](./types/_NewQueryTypeTemplate.md).


## Maintainers <a name="maintainers"> </a> 
This repository is maintained by the [Tellor team](https://github.com/orgs/tellor-io/people).


## Contribute<a name="how2contribute"> </a>  
Join our [discord](https://discord.gg/DxSG2bPECw), help us with issues here on Github, or feel free to reach out anytime [info@tellor.io](mailto:info@tellor.io).


# Copyright

Tellor Inc. 2022
