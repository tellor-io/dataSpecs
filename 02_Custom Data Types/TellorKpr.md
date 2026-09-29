# Automate function calls

## Query Name

- `TellorKpr`

## Query Description
Utilize tellor's oracle to automate function calls by simply submitting a request.  Plus use the flexibility that autopay provides for either one time calls or frequent function calls.  Wether you want this to be done once or more frequently, its simple, all you have to do is submit a one time tip along with the query or set up a datafeed to have it triggered more frequently.

## Query Parameters

This query accepts five parameters: the contract address, the function signature data, the chain id, the timestamp when the function should be triggered, and the max gas cost that should be paid for the call.

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
5. **maxGasFee** (uint256): The max gas amount (priced in the payment token) a keeper can claim along with the tip.

#### Example how to gas convert to payment token
- <details><summary>Math Conversion</summary>

    ```
  max_gas_in_gwei = 400000 gwei
  convert_to_matic = max_gas_in_gwei/1e9 # 0.0004
  convert_gas_to_usd = convert_to_matic*0.59 # 0.59 matic in usd
  payment_token_in_usd = 11 # $11 TRB
  gas_in_trb = convert_gas_to_usd / payment_token_in_usd
  gas_in_trb_e18 = gas_in_trb*1e18
    ```
</details>

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
      "name": "timestampToTrigger",

      "type": "uint256",
      "name": "maxGasFee"
    }
  ],
  "outputs": [
    {
      "type": "(bytes32,address,uint256,uint256)",
      "packed": false
    }
  ]
}
```
## Response Type

The query response will be the transaction hash of the function call transaction, the address of the keeper that first triggered that function after the timestampToTrigger timestamp, the timestamp when the function was triggered, and the gas that was paid by the keeper (in the payment token) to call the function in the following format:

- `abi_type`: `(bytes32,address,uint256,uint256)`
- `packed`: false

## Example

### Tellor Keeper

*Query Descriptor:*

```json
{"type":"TellorKpr","contractAddress":"0xD789488E5ee48Ef8b0719843672Bc04c213b648c","functionData":"0x57806e707c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8db9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000627abef8", "chainId":80001, "timestamp":1652250730, "maxGasFee": 21454545455000}
```

*queryData:*

- <details><summary>Solidity</summary>

    ```javascript
    bytes32 param1 = 0x7c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8d;
    bytes32 param2 = 0xb9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e;
    uint256[] param3 = [1652211448];
    bytes memory _functionData = abi.encodeWithSignature("claimTip(bytes32,bytes32,uint256[])",param1,param2,param3);
    string memory _type = "TellorKpr";
    address _contractAddress = 0xD789488E5ee48Ef8b0719843672Bc04c213b648c;
    uint256 _chainId = 80001;
    uint256 _timestamp = 1652250730;
    uint256 _maxGasFee = 21454545455000;

    _queryData = abi.encode(_type, abi.encode(_contractAddress, _functionData, _chainId, _timestamp, _maxGasFee));
    _queryId = keccak256(_queryData);

    ```

- <details><summary>Python</summary>

    ```python
  function_data_to_bytes = bytes.fromhex("0x57806e707c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8db9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000627abef8"[2:])
    queryDataArgs = encode_abi(["address","bytes","uint256","uint256","uint256"], ["0xD789488E5ee48Ef8b0719843672Bc04c213b648c",function_data_to_bytes,80001,1652250730,21454545455000])
    queryData = encode_abi(["string", "bytes"], ["TellorKpr", queryDataArgs])
    ```

</details>
</details>

` 0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000954656c6c6f724b707200000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000180000000000000000000000000d789488e5ee48ef8b0719843672bc04c213b648c00000000000000000000000000000000000000000000000000000000000000a0000000000000000000000000000000000000000000000000000000000001388100000000000000000000000000000000000000000000000000000000627b586a00000000000000000000000000000000000000000000000000001383468f879800000000000000000000000000000000000000000000000000000000000000a457806e707c9ca4cc348680e2d4637472fc51228a079cb8a6a8cba51fe6f4ebbb3a930c8db9d5e25dabd5f0a48f45f5b6b524bac100df05eaf5311f3e5339ac7c3dd0a37e0000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000627abef800000000000000000000000000000000000000000000000000000000`

*queryID:*

```s
keccak256(queryData)
```

`0x6773c029d9ae7d96706796ef46a6446b31a77bcc85ed2d92f78b6380ab50d1a8`

*Response:*

Once the function is called the transaction hash, keeper's address, and timestamp would be submitted on-chain ie `0x240165a58dc0ace7184e6c49c06a510c7a31f40ebbcc6c5480eb9c98506b2d47,"0xdedc604896be034283365d65118cce2942f99392",1652250736,28190457617000`


*Considerations*

To incentivize reporters/keepers the tip amount should be greater than gas costs.