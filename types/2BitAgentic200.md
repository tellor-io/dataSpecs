## Type Name

`2BitAgentic200`


## Description

The `2BitAgentic200` query type enables general purpose agentic reporting on tellor layer.
Users may create a tip for a `2BitAgentic200`
Each query of this type represents a natural-language
statement which can be easily evaluated as **true**, **false**, or **invalid** (not evaluable).

Agents running the `2bit-reporter` skill's tip listener perform the following workflow as soon as a 2BitAgentic query tip hits the chain:
1. Decode and "normalize" the tipped query statement
2. Detect and filter for indirect prompt injection.
3. If no injection is detected, agent is prompted to evaluate the validity of the user's statement.
4. Reports a verdict of `0` (invalid), `1` (true), or `2` (false).

5. All 2bit data reported to tellor layer is publicly aggregated and checked by the tellor dispute system. All values reported to tellor are disputable for 24 hours, and successful disputers may earn the reporter's stake.

The `200` in the query type name is the `report_block_window`. This query type may be useful as a family convention allowing other block windows to be communicated: `2BitAgentic100` for a 100 block window, `2BitAgentic500` for a 500 block window, etc. Different block windows can be useful for different types of research and more subjective evaluable statements.

Verdict may only be one of three discrete states. The query aggregates
with **weighted-mode** (the state backed by the most total reporter power
wins).

## Query Parameters

The `2BitAgentic200` query type has a single parameter:
```
1. evaluable_statement
    - description: the natural-language statement to be evaluated as true, false, or invalid
    - value type: `string`
```

The statement should be a single, self-contained claim that can be
evaluated without additional context (e.g. `"the sky is blue"`,
`"water boils at 100 C at sea level"`).

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

For the `"the sky is blue"` example, the ABI-encoded query data is:

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000e324269744167656e74696332303000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000f74686520736b7920697320626c75650000000000000000000000000000000000`


## Query ID

The Query ID is the `bytes32` keccak hash of the query data. It uniquely
identifies this query instance.

```s
bytes32 queryId = keccak256(queryData);
```

You can use [this tool](https://tellor.io/queryidstation/) to generate query
IDs.

For the `"the sky is blue"` example, the Query ID is:

`0x752779a8f6f8ff67c2ac001d4a82926ad3fe31a44d0b89afeca8f0d8645c6550`


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

## Example Use Case: Prediction Market

A prediction market is created on September 30th 2026 for whether or not Elon Musk will announce that he is running for president by January 1st 2027. When the market is created, the 2Bit eval statement is recorded with the creation event: "Elon Musk publicly announced that he is running for president in the 2028 election in a press release or X post dated between September 30th 2026 and January 1st 2027" 

When the market expires on January 1st, the user creates a tip with the exact eval statement included as part of the Query Data:

```s
string memory _statement = "Elon Musk publicly announced that he is running for president in the 2028 election in a press release or X post dated between September 30th 2026 and January 1st 2027";
bytes memory _queryData = abi.encode("2BitAgentic200", abi.encode(_statement));
bytes32 _queryId = keccak256(_queryData);
```

- queryData:
  `0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000e324269744167656e7469633230300000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000000a6456c6f6e204d75736b207075626c69636c7920616e6e6f756e63656420746861742068652069732072756e6e696e6720666f7220707265736964656e7420696e20746865203230323820656c656374696f6e20696e20612070726573732072656c65617365206f72205820706f7374206461746564206265747765656e2053657074656d6265722033307468203230323620616e64204a616e756172792031737420323032370000000000000000000000000000000000000000000000000000`
- queryId: `0x6d6aeed0e7ac824156aadc242c8aff1524525cbe11947e64c28d8fd566e52f72`

If the statement is **false** (Musk did not make such an announcement in the
window), the reporter's response is the ABI-encoded value for verdict `2`:

- response: `0x0000000000000000000000000000000000000000000000000000000000000002`


## Dispute Considerations

- **Reporters should not stake more TRB than they are willing to lose**
  **when reporting Tellor 2bit data!**
- A report should be disputed for reporting any verdict that is clearly
  contradicted by well-known, verifiable facts.
- Users should make their statements as clear and self-contained as possible. Vague or
  context-dependent statements may legitimately resolve to "invalid".


## Suggested Data Sources

There is no single canonical API for arbitrary natural-language statements,
so the exact source varies per statement. Users can include suggested sources in the body of their
tip's evaluable statement.
