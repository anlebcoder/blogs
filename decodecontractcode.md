# 解码 JavaScript 合约代码

## 解码某个合约的最新合约代码

这个查看一个工具即可[extract-js-contract](https://github.com/anlebcoder/extract-js-contract)

这个工具可以实时把最新合约的代码解析出来

## 解码历史版本的合约代码

需要经过3步:

- 第一步: 获取历史版本的 setcode 以及 setabi 的data,这些信息在区块链浏览器上有
- 第二步: 通过学习[事务 Transactions 解码知识](./decodebin.md) 解码合约代码
- 第三步：单独解码 setabi 的 data.abi 数据


### 第一步：获取历史版本 code 以及 ABI

看一个数据:

[setcode](https://explorer.fibos.rocks/transactions/376a49c8a7d5310a0aad6c3e53e02537ed78d39081d286dd0f78feed86d049a7) 

数据 data (由于 value 太长，省略了 code 字段):


```
{
  "account": "silver123451",
  "vmtype": 0,
  "vmversion": 0,
  "code": "504b03042d...."
}
```

小提示 code 的value 前缀是 "504b03042d" 则表示是 JavaScript 合约，否则是 C++ 合约。

[setabi](https://explorer.fibos.rocks/transactions/7110e3656b4b0d9baf4b632925a0f47d45a672669e37ebadc518c5d5c59934a7)

数据 data (由于 value 太长，省略了 abi 字段):

```
{
	"account": "silver123451",
	"abi": "0e656f7....."
}
```

### 第二步：解码合约源码

```
const zip = require("zip");
const fs = require("fs");

fs.mkdir("./codes/");

let zipfile = zip.open(Buffer.from(code, "hex")); //code 是 setcode 的 data.code;
zipfile.extractAll('./codes/');
```

### 第三步：解码合约 ABI

```
const FIBOS = require("fibos.js");
const fibos = FIBOS();
let rawdata = FIBOS.modules.Fcbuffer.fromBuffer(fibos.fc.structs.abi_def, Buffer.from(abidata, 'hex'));
console.log(rawdata);
```

输出:
```
{
  "version": "eosio::abi/1.0",
  "types": [],
  .
  .
  .more
}
```
