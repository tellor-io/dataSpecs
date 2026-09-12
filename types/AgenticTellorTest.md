## Type Name

`AgenticTellorTest`


## Description

The `AgenticTellorTest` query type lets a user submit a natural-language
statement and receive a verdict on whether that statement is **true**,
**false**, or **invalid** (not evaluable). The intended reporters are
*agentic* — a Hermes agent (running the `agentic-tellor-reporter` skill)
that reads the statement, researches it (web search and other tools), and
reports a verdict on-chain once it is confident.

Because the verdict is one of three discrete states, the query aggregates
with **weighted-mode** (the state backed by the most total reporter power
wins), not weighted-median.

Typical use case: a user asks a question such as "is the sky blue" and a
downstream consumer (a dApp or a smart contract reading the Tellor oracle)
reads the aggregated boolean verdict.


## Query Parameters

The `AgenticTellorTest` query type has a single parameter:
```
1. statement
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
- true    → `abi.encode(uint8(1))` = `0x0000...0001` (32-byte word, value 1)
- false   → `abi.encode(uint8(2))` = `0x0000...0002` (32-byte word, value 2)
- invalid → `abi.encode(uint8(0))` = `0x0000...0000` (32-byte zero word)


## Query Data

Query data is used to form the Query's unique identifier (query ID) and is
included in emitted contract events so users and reporters can
programmatically construct query objects.

To generate the query data for an instance of an `AgenticTellorTest` query,
first UTF-8 encode the statement. Then ABI-encode it with the query type
string:

```s
string memory _statement = "the sky is blue";
bytes memory _queryDataArgs = abi.encode(_statement);
bytes memory _queryData = abi.encode("AgenticTellorTest", _queryDataArgs);
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
    "type": "AgenticTellorTest",
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


## Example

To evaluate the statement "the sky is blue":

```s
string memory _statement = "the sky is blue";
bytes memory _queryData = abi.encode("AgenticTellorTest", abi.encode(_statement));
bytes32 _queryId = keccak256(_queryData);
```

- queryData:
  `0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000114167656e74696354656c6c6f7254657374000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000f74686520736b7920697320626c75650000000000000000000000000000000000`
- queryId: `0x80de9e88b98377c41e8eec63fe4f58d92c3b06af7c620fce68ae971ddce3a807`

A reporter confident the statement is true submits the value
`0x0000000000000000000000000000000000000000000000000000000000000001`
(abi-encoded `uint8(1)`). If the verdict is false it submits
`0x...0002`, and if the statement is not evaluable it submits
`0x...0000`.

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
