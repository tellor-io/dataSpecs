## Type Name

`AgenticTellorDemo`


## Description

Demo-mode variant of `AgenticTellorTest`: identical ABI, response type, and
aggregation, but with a **short report window** so a full tip -> report ->
aggregate cycle completes inside a live demo (~10 minutes instead of ~90).

Everything else about the query behaves exactly like `AgenticTellorTest`:
a user tips a query whose single argument is a natural-language statement;
agentic reporters research the statement and submit a true/false/invalid
verdict; the aggregate materializes in the EndBlocker when the query
expires.

## Query Parameters

Same single parameter as `AgenticTellorTest`:
```
1. statement
    - description: the natural-language statement to be evaluated as true, false, or invalid
    - value type: `string`
```

Query data = `abi.encode("AgenticTellorDemo", abi.encode(statement))`.
Note the query id is keccak256 of the query data, so it differs from the
`AgenticTellorTest` id for the same statement — the two types are fully
independent queries on-chain.

## Response Type

`uint8` verdict (Tellor boolean-with-invalid convention):
- `0` = invalid
- `1` = true
- `2` = false

```
- abi_type: uint8
- packed: false
```

## Aggregation Method

`weighted-mode` — the verdict backed by the most total reporter power wins.

## Report Window

`report_block_window = 180` blocks (~10 minutes at the devnet's ~3.35 s block
time; the original `AgenticTellorTest` spec uses 2000 blocks).

## Registration

Registered on layertest-8 via `tx registry register-spec` from the registrar
key (`tellor10avkqnhmm7cz7hcltu3tpfyt70z342tduuwkal`) — no gov proposal
needed for a *new* query type:

```json
{"response_value_type":"uint8",
 "abi_components":[{"name":"statement","field_type":"string"}],
 "aggregation_method":"weighted-mode",
 "registrar":"tellor10avkqnhmm7cz7hcltu3tpfyt70z342tduuwkal",
 "report_block_window":"180",
 "query_type":"AgenticTellorDemo"}
```
