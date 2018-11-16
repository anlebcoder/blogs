# FIBOS 事务 Transactions 解码知识

## Transaction 的 hex_data 解码

举个例子,来解码 `eosio.token/excreate`:

[交易地址](https://explorer.fibos.rocks/transactions/89ae5e7ad3a7268f05070f60086c5918eb2f9759f8d53e1e9bf07e0c80c64ea6)

关键数据:

```
 {
  "account": "eosio.token",
  "name": "excreate",
  "authorization": [{
    "actor": "tstmwasd1234",
    "permission": "active"
  }],
  "data": {
    "issuer": "tstmwasd1234",
    "maximum_supply": "1000.0000 GWW",
    "connector_weight": "0.33000000000000002",
    "maximum_exchange": "500.0000 GWW",
    "reserve_supply": "50.0000 GWW",
    "reserve_connector_balance": "40.0000 FO",
    "expiration": "2018-11-16T15:00:00"
  },
  "hex_data": "408608091b2e33ce809698000000000004475757000000001f85eb51b81ed53f404b4c0000000000044757570000000020a10700000000000447575700000000801a06000000000004464f000000000070dbee5b"
 }
```

开始解码:

```
var FIBOS = require("fibos.js");
var fibos = FIBOS({
	chainId: "6aa7bd33b6b45192465afa3553dedb531acaaff8928cf64b70bd4c5e49b7ec6a",
	httpEndpoint: "http://to-rpc.fibos.io:8870",
	logger: {
		log: null,
		error: null
	}
});

var ctx = fibos.contractSync('eosio.token');
let rawdata = FIBOS.modules.Fcbuffer.fromBuffer(ctx.fc.structs.excreate, Buffer.from(hex_data, 'hex'));
console.log("rawdata:", rawdata);
```

输出结果:

```
{
  "issuer": "tstmwasd1234",
  "maximum_supply": "1000.0000 GWW",
  "connector_weight": 0.33,
  "maximum_exchange": "500.0000 GWW",
  "reserve_supply": "50.0000 GWW",
  "reserve_connector_balance": "40.0000 FO",
  "expiration": "2018-11-16T15:00:00"
}
```

解析过程：

```
ctx.fc.structs.excreate 是 excreate 的 ABI action 参数类型

{
	"base": "",
	"action": {
		"name": "excreate",
		"account": "eosio.token"
	},
	"fields": {
		"issuer": "account_name",
		"maximum_supply": "asset",
		"connector_weight": "float64",
		"maximum_exchange": "asset",
		"reserve_supply": "asset",
		"reserve_connector_balance": "asset",
		"expiration": "time_point_sec"
	}
}
```
