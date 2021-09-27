# Tellor Data Specifications

Data specifications for requestID's

## Adding New ID's

Make a PR in this repo to signal to miners that the data is ready to mined and to show them what data they need to put on for a given requestID. 

### Fields:

#### Data Spec ID

    To be filled in by team

#### Request ID: 

    bytes32 request ID or structure for creating the ID (e.g. what to hash)


#### Data Specifications

    Technical Specifications of the value(s) requested


#### Description

    Lamen terms description of the ID and it's implications


#### Granularity

    Multiplier to use (if numeric) for putting on-chain


#### Example Value (w/ date)

    An example piece of data (e.g. "9/27/21 - 9000000")

#### Dispute Considerations

Considerations that parties should consider before reporting the data w/ regard to what will be considered a valid dispute

#### Notes

Any further notes or thoughts on the feed


## Maintainers <a name="maintainers"> </a> 
This repository is maintained by the [Tellor team](https://github.com/orgs/tellor-io/people)


## How to Contribute<a name="how2contribute"> </a>  
Join our Discord or Telegram:
[<img src="./public/telegram.png" width="24" height="24">](https://t.me/tellor)
[<img src="./public/discord.png" width="24" height="24">](https://discord.gg/g99vE5Hb)

Check out our issues log here on Github or feel free to reach out anytime [info@tellor.io](mailto:info@tellor.io)


## Disclaimer  

This is just a guide.  Tellor is decentralized, as with the voting, so nothing is promised.  Mine or use the data at your own risk

Any API examples are only examples and not an endorsement.  It is the reporters job to find valid data sources

Work in a collaborative manner on discord.  If a value is a little stale or a slightly off, be sure to just ping reminders to update or get it off.  It's hard to say what is or is not a good value unless it's painfully obvious, so as the system continues to work out the tweaks, please keep a wide margin of error before starting disputes


It's hard to say "if 10% off" or any hard line value.  Sometimes ETH moves 10% in 5 minutes, so both could be valid.  You really have to use judgement, and even more so you have to realize that disputes are handled by a community consensus, not some formula


## Copyright

Tellor Inc. 2021