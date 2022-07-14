# API Feeds

## Query Name

- `APIQuery`

## Query Description

Use this query type to report numerical values from any API to Tellor oracles. Only use this query type for testnet, as data sources with single points-of-failure shouldn't be used in production.

## Query Parameters

The `APIQuery` type has two parameters, `url` & `parseStr`.

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

## Examples

### Weather Data

*Query Descriptor:*

```json
{"type":"APIQuery","url":"https://samples.openweathermap.org/data/2.5/weather?q=Lond`on,uk&appid=b6907d289e10d714a6e88b30761fae22","key_str":"id, temp_min, clouds"}
```

*queryData:*

```s
abi.encode("APIQuery", abi.encode("https://samples.openweathermap.org/data/2.5/weather?q=Lond`on,uk&appid=b6907d289e10d714a6e88b30761fae22", "id, temp_min, clouds"))
```

`00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000841504951756572790000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000120000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000000e0000000000000000000000000000000000000000000000000000000000000006768747470733a2f2f73616d706c65732e6f70656e776561746865726d61702e6f72672f646174612f322e352f776561746865723f713d4c6f6e64606f6e2c756b2661707069643d623639303764323839653130643731346136653838623330373631666165323200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001469642c2074656d705f6d696e2c20636c6f756473000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`8b2bcace7ce8c4fd4e75cbc1e295151849392f078618d3308017394f15d0e1a6`
     

### Encoding/Decoding

A value of `[[300, 5091, 2643743], 279.15, {'all': 90}]` would be submitted on-chain using the following bytes:

`000000000000000000000000000000000000000000000000000000000000002b5b5b3330302c20353039312c20323634333734335d2c203237392e31352c207b27616c6c273a2039307d5d000000000000000000000000000000000000000000`

### Response Format

This query type searches the entire JSON object returned by the API call for any keys contained in the `key_str` parameter. If there are duplicate keys 
in the dictionary, a list containing all values mapped to that key will be returned. Each key in `key_str` will correspond to an index in the 
returned list, where its values will be stored. 

#### Example 1: Weather
The output of the weather example API is:
```json
{"coord": {"lon": -0.13, "lat": 51.51},
 "weather": [{"id": 300,
   "main": "Drizzle",
   "description": "light intensity drizzle",
   "icon": "09d"}],
 "base": "stations",
 "main": {"temp": 280.32,
  "pressure": 1012,
  "humidity": 81,
  "temp_min": 279.15,
  "temp_max": 281.15},
 "visibility": 10000,
 "wind": {"speed": 4.1, "deg": 80},
 "clouds": {"all": 90},
 "dt": 1485789600,
 "sys": {"type": 1,
  "id": 5091,
  "message": 0.0103,
  "country": "GB",
  "sunrise": 1485762037,
  "sunset": 1485794875},
 "id": 2643743,
 "name": "London",
 "cod": 200}
 ```

The key_str is:`"id, temp_min, clouds"`

The response is: `"[[300, 5091, 2643743], 279.15, {'all': 90}]"`

The first list index holds a list itself of all matching values in the JSON API output that has a key `id`. 
The second has the single value for `temp_min`, and the last contains a dictionary that is the value to the key `clouds`.

#### Example 2: Coindesk Bitcoin Rate

url: `https://api.coindesk.com/v1/bpi/currentprice.json`

API output: 
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

key_str: `"USD, rate"`

response: `[{'code': 'USD', 'symbol': '&#36;', 'rate': '30,075.4176', 'description': 'United States Dollar', 'rate_float': 30075.4176}, ['30,075.4176', '24,088.0937', '28,220.0651']]`

The first index in this list holds the entire dictionary that is the value to the "USD" key. The second index has 
every value to the key "rate" that appears in the entire response. There are multiple ways to access the data to get
the same value from this query type, depending on the API output.


#### Example 3: SpotPrice 

url: `"https://api.coingecko.com/api/v3/simple/price?ids=garlicoin&vs_currencies=usd"`

API output: `{"garlicoin":{"usd":0.02721565}}`

key_str:`"usd"`

response:`[0.02721565]`

Getting a simple spot price for garlicoin, a coin that is unsupported on the larger Tellor SpotPrice query type by directly 
querying an API for its price. Since the output is so simple from the API and only one value is wanted, only one is returned
and requires no further parsing.

## Considerations

Users should use care in selecting data sources and choosing the best way to get desired data.

Due to the inherent security risk of pulling data straight from API's to on chain with no intermediate checks or averaging infrastructure, 
this query type should only be used for testing or for small personal projects.

