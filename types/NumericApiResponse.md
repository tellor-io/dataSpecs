# API with numeric response
## Query Type
- `NumericApiResponse`

## Query Description

Use this query type to report numerical values from any API to Tellor oracles. Only use this query type for testnet, as data sources with single points-of-failure shouldn't be used in production.

## Query Parameters

The `API` query type has two parameters, `url` & `parseStr`.

1. **url**
    - description: API endpoint (e.g ```"https://samples.openweathermap.org/data/2.5/weather?q=Lond`on,uk&appid=b6907d289e10d714a6e88b30761fae22"```)
    - value type: `string`
2. **parseStr**
    - description: Comma-separated string of keys and array indices to access the target numerical value in the JSON response returned from the API endpoint (e.g. `"id, temp_min, clouds"` )
    - value type: `string`
    

## Response Type

The query response will consist of a single 256-bit value in the following format:

- `abi_type`: ufixed256x18 (18 decimals of precision)
- `packed`: false

For example, a reporter decodes an instance of this query to retrieve its `url` and `parseStr` parameter values. The reporter uses the decoded `url` string to retrieve a JSON response from the API in question, then retrieves the target number from that JSON blob with the decoded `parseStr`. The number is 5.25, so the reporter multiplies it by 1e18 for the proper precision, encodes it as bytes, and submits the value to a Tellor oracle.

## Example 1: USD spot price of Garlicoin
Get a simple spot price for garlicoin by querying an API for its price.
**Query parameters and values:**
- url: `"https://api.coingecko.com/api/v3/simple/price?ids=garlicoin&vs_currencies=usd"`
- parseStr: `"garlicoin, usd"`

**API response:** `{"garlicoin":{"usd":0.02721565}}`

Response before applying precision and encoding: `0.02721565`

**queryData:**
(Solidity)
```c
bytes queryData = abi.encode("NumericApiResponse", abi.encode("https://api.coingecko.com/api/v3/simple/price?ids=garlicoin&vs_currencies=usd", "garlicoin, usd"))
```
**queryID:**
(Solidity)
```s
bytes32 queryId = keccak256(queryData)
```

## Example 2: Coindesk Bitcoin USD Rate

url: `https://api.coindesk.com/v1/bpi/currentprice.json`
parseStr: `"bpi, USD, rate_float"`

API response: 
```json
{"time": {"updated": "Jun 2, 2022 12:34:00 UTC", 
"updatedISO": "2022-06-02T12:34:00+00:00", 
"updateduk": "Jun 2, 2022 at 13:34 BST"}, 
"disclaimer": "This data was produced from the CoinDesk Bitcoin Price Index (USD). Non-USD currency data converted using hourly conversion rate from openexchangerates.org", 
"chartName": "Bitcoin", 
"bpi": {"USD": {"code": "USD", "symbol": "&#36;", "rate": "30,075.4176", "description": "United States Dollar", "rate_float": 30075.4176}, 
"GBP": {"code": "GBP", "symbol": "&pound;", "rate": "24,088.0937", "description": "British Pound Sterling", "rate_float": 24088.0937}, 
"EUR": {"code": "EUR", "symbol": "&euro;", "rate": "28,220.0651", "description": "Euro", "rate_float": 28220.0651}}}
```

response before applying precision and encoding: `'30075.4176'`

## Considerations
Don't use this query type in production, since you aren't using multiple sources and since there are no intermediate checks before putting the data on-chain. Only use this query type for testing or small personal projects.

