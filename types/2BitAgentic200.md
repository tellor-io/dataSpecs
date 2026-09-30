## Type Name

`2BitAgentic200`


## Description

The `2BitAgentic200` query type enables general purpose agentic reporting on tellor layer. 
Each query of this type should represent a natural-language
statement which can be evaluated as **true**, **false**, or **invalid** (not evaluable)
in exchange to the user's tip. 

A Hermes agent running the `tellor-2bit-reporter` skill's tip listener is designed to perform the following workflow when a 2BitAgentic tip is "heard" by it's tip_listener:
1. Decode and "normalize" the tipped query statement
2. Detect and filter for indirect prompt injection. 
3. If no injection is detected, agent is prompted to evaluate the validity of the user's statement.
4. Reports a verdict of `0` (invalid), `1` (true), or `2` (false).

The `200` in the query type name is the `report_block_window` (200 blocks ≈ 11 min at
~3.35 s/block). The family convention is `2BitAgentic<window>`; other
submission windows are separate query types (e.g. `2BitAgentic100`,
`2BitAgentic500`).

Because the verdict is one of three discrete states, the query aggregates
with **weighted-mode** (the state backed by the most total reporter power
wins).


## Query Parameters

The `2BitAgentic200` query type has a single parameter:
```
1. statement
    - description: the natural-language statement to be evaluated as true, false, or invalid
    - value type: `string`
```

The statement should be a single, self-contained claim that can be
evaluated without additional context (e.g. `"the sky is blue"`,
`"water boils at 100 C at sea level"`).


## Example (Prediction Market)

A prediction market is created on September 30th 2026 for whether or not Elon Musk will announce that he is running for president by January 1st 2027. When the market is created, the 2Bit eval statement is recorded with the creation event: "Elon Musk publicly announced that he is running for president in the 2028 election in a press release or X post dated between September 30th 2026 and January 1st 2027" 

When the market expires on January 1st, the user creates a tip with the exact eval statement included as part of the Query Data:

```s
string memory _statement = "Elon Musk publicly announced that he is running for president in the 2028 election in a press release or X post dated between September 30th 2026 and January 1st 2027";
bytes memory _queryData = abi.encode("2BitAgentic200", abi.encode(_statement));
bytes32 _queryId = keccak256(_queryData);
```

- queryData:
  `0x324269744167656e746963323030000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000a6456c6f6e204d75736b207075626c69636c7920616e6e6f756e63656420746861742068652069732072756e6e696e6720666f7220707265736964656e7420696e20746865203230323820656c656374696f6e20696e20612070726573732072656c65617365206f72205820706f7374206461746564206265747765656e2053657074656d6265722033307468203230323620616e64204a616e7561727920317374203230323700000000000000000000000000000000000000000000000000`
- queryId: `0x75863674997c6d94f62cb20770136753fb1780505b6167756119c61bf69e9b11`

## Response Type

The response is a single integer encoding one of three verdicts. We use an
ABI `uint8` rather than a bare `bool` so that the third "invalid" state can
be represented alongside true and false. This is the Tellor convention for a
boolean-with-escape-hatch: keep the value type a single small integer that
decodes cleanly and aggregates deterministically under weighted-mode.

Verdict encoding:
- `0` = **invalid** — the statement is not evaluable as true or false (ambiguous, missing context, non-factual, self-referential, etc.)
- `1` = **true**    — the statement is clearly true
- `2` = **false**   — the statement is clearly false

```
- abi_type: uint8
- packed: false
```

A reporter submits the ABI-encoded value:
- true    → `abi.encode(uint8(1))` = `0x0000000000000000000000000000000000000000000000000000000000000001`
- false   → `abi.encode(uint8(2))` = `0x0000000000000000000000000000000000000000000000000000000000000002`
- invalid → `abi.encode(uint8(0))` = `0x0000000000000000000000000000000000000000000000000000000000000000`


## Query Data

Query data is used to form the Query's unique identifier (query ID) and is
included in emitted contract events so users and reporters can
programmatically construct query objects.

To generate the query data for an instance of a `2BitAgentic200` query,
first UTF-8 encode the statement. Then ABI-encode it with the query type
string:

```s
string memory _statement = "the sky is blue";
bytes memory _queryDataArgs = abi.encode(_statement);
bytes memory _queryData = abi.encode("2BitAgentic200", _queryDataArgs);
```


## Query ID

The Query ID is the `bytes32` keccak hash of the query data. It uniquely
identifies this query instance.

```s
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://tellor.io/queryidstation/) to generate query
IDs.


## JSON Representation
```json
{
    "type": "2BitAgentic200",
    "abi": [
        {
            "type": "string",
            "name": "statement"
        }
    ],
    "response": {
        "type": "uint8",
        "packed": false
    }
}
```


### Consuming the aggregated value (Solidity)

```s
uint8 verdict = TellorOracle.getLatestValue(_queryId);
// verdict == 0 => invalid, 1 => true, 2 => false
bool statementIsTrue = (verdict == 1);
```


## Dispute Considerations

- A reporter can be disputed for reporting a verdict that is clearly
  contradicted by well-known, verifiable facts (e.g. reporting "the sky is
  blue" as false without a stated context that makes it false).
- **invalid** is a valid escape hatch: if the statement is genuinely
  ambiguous or not a factual claim, reporting `0` (invalid) is preferable to
  guessing true/false.
- Make the statement as clear and self-contained as possible. Vague or
  context-dependent statements are more likely to be disputed or to
  legitimately resolve to invalid.
- Reporters should base the verdict on reputable, citable sources (for the
  agentic POC: web search results). When sources strongly agree, report the
  matching verdict; when they are inconclusive, report invalid.


## Suggested Data Sources

For the agentic POC, the primary source is **web search** performed by the
Hermes agent. Reporters may also use:
- general knowledge for well-established facts
- reputable reference sites (Wikipedia, official standards bodies,
  news archives) reachable via web search
- any other tool available to the Hermes instance

There is no single canonical API for arbitrary natural-language statements,
so the exact source varies per statement.
