## Query Name

- `CrossChainBalance`

## Query Description

This query returns the merke root for a given merkle tree of a token's balances. Reporters will create a Merkle Tree of all addresses and their balances at a certain block/timestamp and then upload the bytes32 merkle root.

## Query Parameters

The `CrossChainBalance` query has three parameters, which specifies the requested data.

1. **chainId** (uint256):

The `chainId` should be a valid EVM chain ID.

2. **contractAddress** (address):

The `contractAddress` should be a valid token contract on the given EVM chain.

3. **timestamp** (uint256):

The timestamp is a valid timestamp on the chainId specified.  The block should correspond to first block before or equal to the timestamp. 

## Response Type

The query response will consist of a boolean value in the following format:

- `abi_type`: bytes32
- `packed`: false

*Query Descriptor:*

```json
{
  "type": "CrossChainBalance",
  "inputs": [
    {
      "type": "uint256",
      "name": "chainId"
    },
    {
      "type": "address",
      "name": "contractAddress"
    },
    {
      "type": "uint256",
      "name": "timestamp"
    }
  ],
  "outputs": [
    {
      "type": "bytes32",
      "packed": false
    }
  ]
}
```

*queryData:*

```s
abi.encode("CrossChainBalance", abi.encode(1,0x88dF592F8eb5D7Bd38bFeF7dEb0fBc02cf3778a0,1668794903))
```

`0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000001143726f7373436861696e42616c616e63650000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000060000000000000000000000000000000000000000000000000000000000000000100000000000000000000000088df592f8eb5d7bd38bfef7deb0fbc02cf3778a00000000000000000000000000000000000000000000000000000000000f41e7e`

*queryID:*

```s
keccak256(abi.encode("CrossChainBalance", abi.encode(1,0x88dF592F8eb5D7Bd38bFeF7dEb0fBc02cf3778a0,1668794903)))
```

`0xaa26b2933bddaf789f17ea619cdbe598e31939b531a81bdf9241970c5e9f5ecf`


## Calculating

An example repo for generating the merkle root can be found here:  https://github.com/tellor-io/crosschainBalances


## Dispute Considerations

- It is the reporters responsibility to calculate out the correct merkle root.  


## Example

In the example query:  

```
queryData = abi.encode("CrossChainBalance", abi.encode(1,0x88dF592F8eb5D7Bd38bFeF7dEb0fBc02cf3778a0,1668794903))

```

the correct response is: 

```
0x3b696cbaa12880500df23f90cf5599987649df71fe24e830cc21fbb95891dbe7

``


