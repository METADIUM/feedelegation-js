## Examples for fee delegation transaction

***
Fee Delegation Transaction is a service where the feePayer pays the fee for the transaction that the Sender wants to execute. This is done by adding the feePayer's signature to the existing transaction information signed by the Sender and sending it. Fee Delegation Transaction only supports the DynamicFeeTxType among the existing transactions signed by the Sender and does not support LegacyTxType or AccessListTxType.

|      Tx Type       | Tx Type supported with fee delegation |
|:------------------:|---------------------------------------|
|    LegacyTxType    | No                                    |
|  AccessListTxType  | No                                    |
|  DynamicFeeTxType  | Yes                                   |

***
### 1. Examples using CURL command

Firstly, create raw data of DynamicFeeTxType(0x02) transaction using personal_signTransaction method.

```
curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"personal_signTransaction","params":[{"from":"0xc3e44aac2d0457942baffa1dc3be313bc8d65627","to":"0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f","nonce":"0x26","value":"0xDE0B6B3A7640000","maxPriorityFeePerGas":"0x174876E801","maxFeePerGas":"0x174876E801","gas":"0x5208"},"test"],"id":1}' http://127.0.0.1:9588

result: {"jsonrpc":"2.0","id":1,"result":{"raw":"0x02f8740c2685174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a06ad4753f48d3cad0b3b5ada1d03718a6abd81467a9896e63c012c944c9014a5ba0163a2aadee9028dc2c2993c55e2d519b58345832a6b540f3b0c71697f25aabfc","tx":{"type":"0x2","nonce":"0x26","gasPrice":null,"maxPriorityFeePerGas":"0x174876e801","maxFeePerGas":"0x174876e801","gas":"0x5208","value":"0xde0b6b3a7640000","input":"0x","v":"0x1","r":"0x6ad4753f48d3cad0b3b5ada1d03718a6abd81467a9896e63c012c944c9014a5b","s":"0x163a2aadee9028dc2c2993c55e2d519b58345832a6b540f3b0c71697f25aabfc","to":"0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f","chainId":"0xc","accessList":[],"hash":"0x6868e5da54e47bd09c6c5cb73a9f32c23db059d2873f3112bc37dd83824c4b4a"}}}
``` 

Secondly, create raw data of fee delegation transaction using personal_signRawFeeDelegateTransaction method (which is added to support fee delegation transaction).

```
curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"personal_signRawFeeDelegateTransaction","params":[{"feePayer":"0xd5d06d0d1ec47131e284ac3a88864fb750dba9f6"},"0x02f8740c2685174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a06ad4753f48d3cad0b3b5ada1d03718a6abd81467a9896e63c012c944c9014a5ba0163a2aadee9028dc2c2993c55e2d519b58345832a6b540f3b0c71697f25aabfc","test"],"id":1}' http://127.0.0.1:9588

result: {"jsonrpc":"2.0","id":1,"result":{"raw":"0x16f8cef8740c2685174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a06ad4753f48d3cad0b3b5ada1d03718a6abd81467a9896e63c012c944c9014a5ba0163a2aadee9028dc2c2993c55e2d519b58345832a6b540f3b0c71697f25aabfc94d5d06d0d1ec47131e284ac3a88864fb750dba9f680a042cfb84cd7724d648b79a6a486b5e632842295d7be4761165860abbb2c34ec15a01e60339ae50691dbbe290d5c7739e1d75271a316cd643effe247ab6514480b05","tx":{"type":"0x16","nonce":"0x26","gasPrice":null,"maxPriorityFeePerGas":"0x174876e801","maxFeePerGas":"0x174876e801","gas":"0x5208","value":"0xde0b6b3a7640000","input":"0x","v":"0x1","r":"0x6ad4753f48d3cad0b3b5ada1d03718a6abd81467a9896e63c012c944c9014a5b","s":"0x163a2aadee9028dc2c2993c55e2d519b58345832a6b540f3b0c71697f25aabfc","to":"0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f","chainId":"0xc","accessList":[],"hash":"0x0c04b9b1366135a4ea6d392075c896fbf82c4af0e7bf83656f28d1d24ace5e31","feePayer":"0xd5d06d0d1ec47131e284ac3a88864fb750dba9f6","fv":"0x0","fr":"0x42cfb84cd7724d648b79a6a486b5e632842295d7be4761165860abbb2c34ec15","fs":"0x1e60339ae50691dbbe290d5c7739e1d75271a316cd643effe247ab6514480b05"}}}
``` 

Finally, send signed fee delegation transaction to blockchain.

```
curl -H "Content-Type: application/json" -X POST --data '{"jsonrpc":"2.0","method":"eth_sendRawTransaction","params":["0x16f8cef8740c2685174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a06ad4753f48d3cad0b3b5ada1d03718a6abd81467a9896e63c012c944c9014a5ba0163a2aadee9028dc2c2993c55e2d519b58345832a6b540f3b0c71697f25aabfc94d5d06d0d1ec47131e284ac3a88864fb750dba9f680a042cfb84cd7724d648b79a6a486b5e632842295d7be4761165860abbb2c34ec15a01e60339ae50691dbbe290d5c7739e1d75271a316cd643effe247ab6514480b05"],"id":1}' http://127.0.0.1:9588

{"jsonrpc":"2.0","id":1,"result":"0x0c04b9b1366135a4ea6d392075c896fbf82c4af0e7bf83656f28d1d24ace5e31"}
``` 
#### personal_signRawFeeDelegateTransaction method

parameters:

    fee delegation data : {"feePayer":"0xd5d06d0d1ec47131e284ac3a88864fb750dba9f6"},

    DynamicFeeTxType raw data : "0x.....",

    feePayer wallet passwd : "test"

result:

    fee delegation transaction raw data : "0x..."
***
### 2. Examples using gmet console

Firstly, create raw data of DynamicFeeTxType(0x02) transaction using personal.signTransaction function.
```
$ bin/gmet.sh console
> personal.signTransaction({"from":"0xc3e44aac2d0457942baffa1dc3be313bc8d65627","to":"0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f","nonce":"0x27","value":"0xDE0B6B3A7640000","maxPriorityFeePerGas":"0x174876E801","maxFeePerGas":"0x174876E801","gas":"0x5208"})

{
  raw: "0x02f8740c2785174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a01a507c62a223da0fc4c6a021f55fd7cf74b97f195c4984376cc0596be9bcb444a06c1d511541f854a8bea54cd9620aa70690275d272d2c3e6e5ccfed0a05f18e7d",
  tx: {
    accessList: [],
    chainId: "0xc",
    gas: "0x5208",
    gasPrice: null,
    hash: "0x9b091c4580e6fb06e7e738a890d28cfeeadb2e27e218f2a742f074523654fceb",
    input: "0x",
    maxFeePerGas: "0x174876e801",
    maxPriorityFeePerGas: "0x174876e801",
    nonce: "0x27",
    r: "0x1a507c62a223da0fc4c6a021f55fd7cf74b97f195c4984376cc0596be9bcb444",
    s: "0x6c1d511541f854a8bea54cd9620aa70690275d272d2c3e6e5ccfed0a05f18e7d",
    to: "0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f",
    type: "0x2",
    v: "0x1",
    value: "0xde0b6b3a7640000"
  }
}
```

Secondly, create raw data of fee delegation transaction using personal.signTransaction function (which is added to support fee delegation transaction).
```
$ bin/gmet.sh console
> personal.signRawFeeDelegateTransaction({"feePayer":"0xd5d06d0d1ec47131e284ac3a88864fb750dba9f6"},"0x02f8740c2785174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a01a507c62a223da0fc4c6a021f55fd7cf74b97f195c4984376cc0596be9bcb444a06c1d511541f854a8bea54cd9620aa70690275d272d2c3e6e5ccfed0a05f18e7d","test")

{
  raw: "0x16f8cef8740c2785174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a01a507c62a223da0fc4c6a021f55fd7cf74b97f195c4984376cc0596be9bcb444a06c1d511541f854a8bea54cd9620aa70690275d272d2c3e6e5ccfed0a05f18e7d94d5d06d0d1ec47131e284ac3a88864fb750dba9f680a0f35500f26ccfb8e120736911edf6afabe0aca867a9fbd008bf9937cf8f88d630a02451dddaf34bce585fa9b73df0a8016439aca38e72de9e971a805f85252407f4",
  tx: {
    accessList: [],
    chainId: "0xc",
    feePayer: "0xd5d06d0d1ec47131e284ac3a88864fb750dba9f6",
    fr: "0xf35500f26ccfb8e120736911edf6afabe0aca867a9fbd008bf9937cf8f88d630",
    fs: "0x2451dddaf34bce585fa9b73df0a8016439aca38e72de9e971a805f85252407f4",
    fv: "0x0",
    gas: "0x5208",
    gasPrice: null,
    hash: "0x1ecbdb79579bbd527893539e6ee7a4203654a1b4feb809d32647317c244588da",
    input: "0x",
    maxFeePerGas: "0x174876e801",
    maxPriorityFeePerGas: "0x174876e801",
    nonce: "0x27",
    r: "0x1a507c62a223da0fc4c6a021f55fd7cf74b97f195c4984376cc0596be9bcb444",
    s: "0x6c1d511541f854a8bea54cd9620aa70690275d272d2c3e6e5ccfed0a05f18e7d",
    to: "0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f",
    type: "0x16",
    v: "0x1",
    value: "0xde0b6b3a7640000"
  }
}
```

Finally, send signed fee delegation transaction to blockchain using eth.sendRawTransaction function.
```
$ bin/gmet.sh console
> eth.sendRawTransaction('0x16f8cef8740c2785174876e80185174876e80182520894db8408bb47bf5e745fed00fc2c99e2f4e1a9270f880de0b6b3a764000080c001a01a507c62a223da0fc4c6a021f55fd7cf74b97f195c4984376cc0596be9bcb444a06c1d511541f854a8bea54cd9620aa70690275d272d2c3e6e5ccfed0a05f18e7d94d5d06d0d1ec47131e284ac3a88864fb750dba9f680a0f35500f26ccfb8e120736911edf6afabe0aca867a9fbd008bf9937cf8f88d630a02451dddaf34bce585fa9b73df0a8016439aca38e72de9e971a805f85252407f4')

0x1ecbdb79579bbd527893539e6ee7a4203654a1b4feb809d32647317c244588da
```
#### * personal.signRawFeeDelegateTransaction() RPC

 parameters:

    fee delegation data : {"feePayer":"0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f"},

    DynamicFeeTxType raw data : "0x.....",

    feePayer wallet passwd : "test"

result:

    fee delegation transaction raw data : "0x..."
***
### 3. Examples using JavaScript

To send fee delegation transaction, we provide two utility modules (FeeDelegateDecode.js and FeeDelegateEncode.js).
And these will need web3.js and ethereumjs-util.js libraries.

#### usage:

##### 1. Install node js.(https://nodejs.org/ko/download)

##### 2. Install web3.js and ethereumjs-util.js using npm.
```
npm install web3; npm install ethereumjs-util;
```

##### 3. Save FeeDelegateDecode.js,FeeDelegateEncode.js and FeeDelegateSampleCode.js in same directory.

##### 4. Execute FeeDelegateSampleCode.js using node command.(Caution: You must prepare available wallet address and private key, and deposit corresponding amount of META in advance.)
```
$ node FeeDelegateSampleCode.js

{
  messageHash: '0xefd65be585e795486c3f0de64c9facfb7facc536f672a55b603e030adcc12808',
  v: '0x0',
  r: '0x663c274612c6478e06e8a2ba53741ac73c33671555f278523a1ab3d6a5888fd4',
  s: '0x5c562792a2c99f5919dc71280eef43a0fb3184a24c4d8c595aeb22710b4944b3',
  rawTransaction: '0x02f86f0c8085174876e80185174876e801830186a09482667998ae5fd9e4f4637fc805e97740c673c51782010080c080a0663c274612c6478e06e8a2ba53741ac73c33671555f278523a1ab3d6a5888fd4a05c562792a2c99f5919dc71280eef43a0fb3184a24c4d8c595aeb22710b4944b3',
  transactionHash: '0x139aeff6dae4e3cb49535aeb19e8f1e09393e3d550d14b75cc47334282869269'
}
decodeRLP= 0x02
Unsigned FDDTx= {
  chainId: 12,
  nonce: 0,
  maxPriorityFeePerGas: '0x174876e801',
  maxFeePerGas: '0x174876e801',
  gas: '0x0186a0',
  to: '0x82667998ae5fd9e4f4637fc805e97740c673c517',
  value: '0x0100',
  input: '0x',
  accessList: [],
  v: <Buffer >,
  r: <Buffer 66 3c 27 46 12 c6 47 8e 06 e8 a2 ba 53 74 1a c7 3c 33 67 15 55 f2 78 52 3a 1a b3 d6 a5 88 8f d4>,
  s: <Buffer 5c 56 27 92 a2 c9 9f 59 19 dc 71 28 0e ef 43 a0 fb 31 84 a2 4c 4d 8c 59 5a eb 22 71 0b 49 44 b3>,
  feePayer: '0xdb8408bb47bf5e745fed00fc2c99e2f4e1a9270f'
}

....
....
```


- Please refer to the following for details.
  - sample code : FeeDelegateSampleCode.js
  - git : https://github.com/METADIUM/feedelegation-js.git


- go-metadium : https://github.com/METADIUM/go-metadium.git
