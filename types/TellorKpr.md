# Automate function calls

## Query Name

- `TellorKpr`

## Query Description
Utilize tellor's oracle to automate function calls by simply submitting a request.  Plus use the flexibility that autopay provides for either one time calls or frequent function calls.  Wether you want this to be done once or more frequently, its simple, all you have to do is submit a one time tip along with the query or set up a datafeed to have it triggered more frequently.

## Query Parameters

This query accepts four parameters: the contract address, the function signature data, the chain id, and the timestamp when the function should be triggered.

1. **contractAddress** (address): The contract address where the function to be called lives. (e.g. Tellor Autopay Address: [0xD789488E5ee48Ef8b0719843672Bc04c213b648c](https://mumbai.polygonscan.com/address/0xD789488E5ee48Ef8b0719843672Bc04c213b648c))
2. **functionData** (bytes): The bytes encoding of the function and its parameters' signature. (e.g. bytes.fromhex('57806e707c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8db9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000627abef8'))

:information_source: functionData is an encoding of the function and its parameters if any. So its important that the function is encoded with the appropriate parameters!

#### Example in python to get function and params calldata
- <details><summary>Python</summary>

    ```python
  feed_id =  bytes.fromhex("0x7c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8d"[2:])
  query =bytes.fromhex("0xb9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e"[2:])
  time_stamp = [1652211448]
  call_data = autopay_contract.encodeABI(fn_name='claimTip',args=[feed_id,query,time_stamp])
    ```

</details>

3. **chainId** (uint256): The chain id where the contract lives. (e.g. 80001)
4. **timestampToTrigger** (uint256): The timestamp the function should be triggered on.

*Descriptor:*
```json
{
  "type": "TellorKpr",
  "inputs": [
    {
      "type": "bytes",
      "name": "functionData",
      
      "type": "address",
      "name": "contractAddress",

      "type": "uint256",
      "name": "chainId",

      "type": "uint256",
      "name": "timestampToTrigger"
    }
  ],
  "outputs": [
    {
      "type": "(bytes32,address,uint256)",
      "packed": false
    }
  ]
}
```
## Response Type

The query response will be the transaction hash of the function call transaction, the address of the keeper that first triggered that function after the timestampToTrigger timestamp and the timestamp when the function was triggered in the following format:

- `abi_type`: `(bytes32,address,uint256)`
- `packed`: false

## Example

### Tellor Keeper

*Query Descriptor:*

```json
{"type":"TellorKpr","contractAddress":"0xD789488E5ee48Ef8b0719843672Bc04c213b648c","functionData":"0x57806e707c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8db9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000627abef8", "chainId":80001, "timestamp":1652250730}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    abi.encode("TellorKpr", abi.encode(0xD789488E5ee48Ef8b0719843672Bc04c213b648c,b'W\x80np|\x9c\xa4\xcc4\x86\x80\xe2\xd4ctr\xfcQ"\x8a\x07\x9c\xb8\xa6\xa8\xcb\xa5\x1f\xe6\xf4\xeb\xbb:\x93\x0c\x8d\xb9\xd5\xe2]\xab\xd5\xf0\xa4\x8fE\xf5\xb6\xb5$\xba\xc1\x00\xdf\x05\xea\xf51\x1f>S9\xac|=\xd0\xa3~\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00`\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x01\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00bz\xbe\xf8',80001,1652250730))
    ```

- <details><summary>Python</summary>

    ```python
  function_data_to_bytes = bytes.fromhex("0x57806e707c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8db9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000627abef8"[2:])
    queryDataArgs = encode_abi(["address","bytes","uint256","uint256"], ["0xD789488E5ee48Ef8b0719843672Bc04c213b648c",function_data_to_bytes,80001,1652250730])
    queryData = encode_abi(["string", "bytes"], ["TellorKpr", queryDataArgs])
    ```

</details>
</details>

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000954656c6c6f724b707200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000160x000000000000000000000000d789488e5ee48ef8b0719843672bc04c213b648c0000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000001388100000000000000000000000000000000000000000000000000000000627b586a00000000000000000000000000000000000000000000000000000000000000a457806e707c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8db9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000627abef800000000000000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0x29af57b4cfa993ceec256a61380107cccd499a99ee38519506bfc6fc88103970`

*Response:*

Once the function is called the transaction hash, keeper's address, and timestamp would be submitted on-chain ie `0x240165a58dc0ace7184e6c49c06a510c7a31f40ebbcc6c5480eb9c98506b2d47,"0xdedc604896be034283365d65118cce2942f99392",1652250736`


*Considerations*

To incentivize reporters/keepers the tip amount should be greater than gas costs.