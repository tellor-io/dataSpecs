## Query Name

- `ExampleNftCollectionStats`

## Query Description

This query is ideal for a hackathon/demo/mvp project. Consider a more robust data spec for production, as it's risky to base smart contract logic on NFT collection price stats like floor prices when market manipulation / wash trading happen frequently.

## Query Parameters

The `ExampleNftCollectionStats` query has one parameter:

1. **collectionSlug**:
    - description: unique name of the NFT collection
    - value type: `string`

## Response Type

Solidity type:
- `abi_type`: `uint256[]`
- `packed`: false

The query response will consist of an array of integers. Each integer represents the original value to 18 decimals of precision (you multiply the original `float` by 10**18). The array includes the floor price, total volume, and best offer, in that order.

So say you wanted to report stats for the [Moonbirds collection](https://opensea.io/collection/proof-moonbirds). Add the floor price (11.0 ETH), total volume (171100.0 ETH), and best offer (10.0 ETH) to an array and encode that as the expected Solidity response type. You could get more accurate stats from an API like [Opensea's](https://docs.opensea.io/reference/retrieving-collection-stats). Again, for production you'd want require reporters to use more than one data source.

An example of encoding this query type's response in Python:
```python
from eth_abi import encode_abi

data = [int(stat * 10**18) for stat in (11.0, 171100.0, 10.0)]
submit_value_bytes = encode_abi(["uint256[]"], [data])
print(submit_value_bytes.hex())
# 0x0000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000300000000000000000000000000000000000000000000000098a7d9b8314c000000000000000000000000000000000000000000000000243b597e7159180000000000000000000000000000000000000000000000000000008ac7230489e80000
```

And decoding:
```python
from eth_abi import decode_abi

decoded = decode_abi(["uint256[]"], submit_value_bytes)
print(decoded.hex())
# ((11000000000000000000, 171100000000000001048576, 10000000000000000000),)
# ignore Python's inaccurate precision for floats
```

## Example queryData and queryId
(Solidity)

_queryData:_

```js
// First encode the query parameter, "proof-moonbirds", for the collection slug name
// Then encode those bytes with the query type string, "ExampleNftCollectionStats"
abi.encode("ExampleNftCollectionStats", abi.encode("proof-moonbirds"))
```

`0x0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000194578616d706c654e6674436f6c6c656374696f6e53746174730000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000f70726f6f662d6d6f6f6e62697264730000000000000000000000000000000000`

_queryID:_

```js
// Generate the unique identifier for your data, this query instance, by taking the keccak hash of the query data (generated above)
keccak256(abi.encode("ExampleNftCollectionStats", abi.encode("proof-moonbirds")))
```

`0xe01a8d6c7904ee37aadcc5b7086a1290d0861f1b9a93c08c7ade5ca1d7bb6f06`

## Decoding submitted value in solidity
More Solidity help [here](https://tellor.io/docs/).

```js
pragma solidity >=0.8.0;

import "usingtellor/contracts/UsingTellor.sol";

contract MyContract is UsingTellor {

  constructor(address payable _tellorAddress) UsingTellor (_tellorAddress) public {}

  function getCollectionFloorPrice() external view returns(uint256) {
    
      bytes memory _queryData = abi.encode("ExampleNftCollectionStats", abi.encode("proof-moonbirds"));
      bytes32 _queryId = keccak256(_queryData);
      
      (bool ifRetrieve, bytes memory _value, ) =
          getDataBefore(_queryId, block.timestamp - 1 hours);
      if (!ifRetrieve) return 0;
      // Returns moon bird floor price, 11 * 10**18
      return abi.decode(_value, (uint256[]))[0];
    }

}
```
