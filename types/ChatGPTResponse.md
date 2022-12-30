### Contact Details

t.j.wies@gmail.com

### Query Type Name

ChatGPTResponse

### Description

This query type will be used to put ChatGPT responses to prompts on chain for use by other smart contracts.
It will accept a `string memory`, its prompt, and return a `string memory`, its GPTchat answer.

### Your query parameters

- prompt
    - description: the prompt the reporter will put in ChatGPT
    - value type: `string memory`

### Your response type

response type: `string memory`

### Example

#### Query Data

```s
queryData = abi.encode("ChatGPTResponse", abi.encode(bytes("What is Tellor?")))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000f43686174475054526573706f6e7365000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000f576861742069732054656c6c6f723f0000000000000000000000000000000000`

#### QueryId

The query id is the keccak256 hash of the query data:

```s
keccak256(queryData)
```

`0x2ea03b01e00fcca8272a9e05aa0373ccdfdeddcbc8fe63eaf7cf62ca147568f5`


