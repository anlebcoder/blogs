# 解码 block 块数据的 packed_trx

与上面的原理一样，找一个例子:


```
packed_trx: "aef9f45b1f00093f61fe00000000030000000000ea305500409e9a2264b89a0100000000004c8f5b00000000a8ed32326600000000004c8f5b5052ad27ab99b1ca010000000100030d15411b92d3ec8ab04872e02beda424c20f93b9a3b3f12b69d7c07e308a5ff201000000010000000100030d15411b92d3ec8ab04872e02beda424c20f93b9a3b3f12b69d7c07e308a5ff2010000000000000000ea305500b0cafe4873bd3e0100000000004c8f5b00000000a8ed32321400000000004c8f5b5052ad27ab99b1ca000020000000000000ea305500003f2a1ba6a24a0100000000004c8f5b00000000a8ed32323100000000004c8f5b5052ad27ab99b1caa08601000000000004464f0000000000a08601000000000004464f00000000000100"
```

解码：

```
const FIBOS = require("fibos.js");
const fibos = FIBOS();
let rawdata = FIBOS.modules.Fcbuffer.fromBuffer(fibos.fc.structs.transaction, Buffer.from(packed_trx, 'hex'));
console.log(rawdata);
```

输出结果：

根据结果可以看到这是一个创建账户的 Transactions。

```
{
  "expiration": "2018-11-21T06:22:38",
  "ref_block_num": 31,
  "ref_block_prefix": 4267785993,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [
    {
      "account": "eosio",
      "name": "newaccount",
      "authorization": [
        {
          "actor": "fibos",
          "permission": "active"
        }
      ],
      "data": "00000000004c8f5b5052ad27ab99b1ca010000000100030d15411b92d3ec8ab04872e02beda424c20f93b9a3b3f12b69d7c07e308a5ff201000000010000000100030d15411b92d3ec8ab04872e02beda424c20f93b9a3b3f12b69d7c07e308a5ff201000000"
    },
    {
      "account": "eosio",
      "name": "buyrambytes",
      "authorization": [
        {
          "actor": "fibos",
          "permission": "active"
        }
      ],
      "data": "00000000004c8f5b5052ad27ab99b1ca00002000"
    },
    {
      "account": "eosio",
      "name": "delegatebw",
      "authorization": [
        {
          "actor": "fibos",
          "permission": "active"
        }
      ],
      "data": "00000000004c8f5b5052ad27ab99b1caa08601000000000004464f0000000000a08601000000000004464f000000000001"
    }
  ],
  "transaction_extensions": []
}
```