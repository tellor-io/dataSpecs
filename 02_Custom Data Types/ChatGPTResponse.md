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

#### Response

Response type: string

Example response: `bytes response = 0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000020454656c6c6f72206973206120646563656e7472616c697a6564204f7261636c6520706c6174666f726d207468617420616c6c6f777320757365727320746f207365637572656c7920616e64207472616e73706172656e746c79207265717565737420646174612066726f6d2065787465726e616c20736f75726365732e2054686520706c6174666f726d20757365732061206e6574776f726b206f66206d61737465726e6f64657320746f2067617468657220616e642076616c696461746520646174612c207768696368206973207468656e2073746f726564206f6e2074686520457468657265756d20626c6f636b636861696e2e2054656c6c6f722069732064657369676e656420746f2062652061206d6f72652073656375726520616e642072656c6961626c6520616c7465726e617469766520746f20747261646974696f6e616c2063656e7472616c697a6564204f7261636c65732c207768696368206172652076756c6e657261626c6520746f2074616d706572696e6720616e642063656e736f72736869702e2054656c6c6f72206973207573656420696e20612076617269657479206f66206170706c69636174696f6e732c20696e636c7564696e672070726564696374696f6e206d61726b6574732c20736d61727420636f6e7472616374732c20616e642066696e616e6369616c206170706c69636174696f6e732e00000000000000000000000000000000000000000000000000000000`

Decoded with `abi.decode(response, (string))`:

`Tellor is a decentralized Oracle platform that allows users to securely and transparently request data from external sources. The platform uses a network of masternodes to gather and validate data, which is then stored on the Ethereum blockchain. Tellor is designed to be a more secure and reliable alternative to traditional centralized Oracles, which are vulnerable to tampering and censorship. Tellor is used in a variety of applications, including prediction markets, smart contracts, and financial applications.`


### Dispute considerations

New lines should be stripped. In other words there should not be separate paragraphs in the on-chain response.
Responses should include the major keywords of the question to be considered valid. Users of this query type should also examine the answer before accepting it in their smart contract, as irrelevant or off-topic answers can be hard to detect.

### Suggested Data Sources

Data source should be manual entry: a copied response from ChatGPT pasted into Etherscan, Telliot, or another reporting interface.




