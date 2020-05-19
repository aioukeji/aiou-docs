---
id: AIOU-class
title: AIOU
sidebar_label: AIOU
---

AIOU类负责创建交易，同时也提供钱包hook入口

## constructor
构造函数

### Parameters
Name             |Type       |Description 
----                |--         |--
config |Object         | config object 当前 AIOU 使用的配置，详细如下:<br/> <b>gasRatio:</b> 交易的gas倍率 <br/> <b>gasLimit:</b> 交易的gas limit <br/> <b>expiration:</b> 交易过期时间，以秒记

### Returns
AIOU实例

### Example
```javascript
// init aiou sdk
const aiou = new AIOU.AIOU({ // will use default setting if not set
    gasRatio: 1,
    gasLimit: 100000,
    expiration: 90,
});
```

## callABI
产生一个调用ABI的交易

### Parameters
Name             |Type       |Description 
----                |--         |--
contract |String         | 合约ID或者合约域名
abi 	 |String 		 | 合约ABI
args	 |Array			 | 参数的数组

### Returns
交易实例

### Example
```javascript
const tx = aiou.callABI(
	"token.aiou",
	"transfer",
	["aiou", "fromAccount", "toAccount", "10.000", "memo"]
);
```


## newAccount
新建账户

### Parameters
Name             |Type       |Description 
----                |--         |--
name 			 |String	| 账户名
creator 	 	 |String	| 创建者的账户名
ownerkey	 	 |String	| 新建账户的owner公钥
activekey	 	 |String	| 新建账户的active公钥
initialRAM	 	 |Number	| 初始购买的RAM，由创建者付费
initialGasPledge |Number	| 初始抵押的AIOU，由创建者付费

### Returns
交易实例

### Example
```javascript
// first create KeyPair for new account
const newKP = KeyPair.newKeyPair();
// then create new Account transaction
const newAccountTx = aiou.newAccount(
    "test1",
    "admin",
    newKP.id,
    newKP.id,
    1024,
    10
);
```

## transfer
转账token

### Parameters
Name             |Type       |Description 
----                |--         |--
token		|String	| token的名字
from 	 	|String	| 转出账户
to			|String	| 转入账户
amount	 	|String	| 金额
memo	 	|Number	| 备注

### Returns
交易实例

### Example
```javascript
const tx = aiou.transfer("aiou", "fromAccount", "toAccount", "10.000", "memo");
```

# 如何发送交易
使用signAndSend可以直接发送交易，并且能够无缝衔接到第三方钱包

```
aiou.setAccount(account);
aiou.setRPC(rpc);
aiou.signAndSend(tx)
    .on("pending", console.log)
    .on("success", console.log)
    .on("failed", console.log);
```

第三方钱包可以hook AIOU类，接管以上3个函数, 就可以无缝衔接到现有的逻辑当中。

当第三方钱包存在时，signAndSend将会把tx发送到第三方钱包处理，而不存在时，则会通过默认l5uoji使用```rpc```和```account```发送交易。

## signAndSend
签名并且发送交易.

### Parameters
Name             |Type       |Description 
----                |--         |--
tx       |AIOU.Tx | 待发送的交易。

### Returns
Callback实例

### Example
```javascript
aiou.signAndSend(tx)
    .on("pending", console.log)
    .on("success", console.log)
    .on("failed", console.log);
```

## setAccount
设置AIOU使用的accunt

### Parameters
Name             |Type       |Description 
----                |--         |--
account       |AIOU.Account | 当前aiou实例

### Returns
null

### Example
```javascript
aiou.setAccount(account);
```

## currentAccount
获取AIOU当前的account

### Parameters
null

### Returns
AIOU.Account.

### Example
```javascript
const account = aiou.currentAccount();
```

## setRPC
设置当前aiou实例使用的rpc

### Parameters
Name             |Type       |Description 
----                |--         |--
rpc       |AIOU.RPC | 设置当前aiou实例使用的rpc

### Returns
null.

### Example
```javascript
aiou.setRPC(rpc);
```

## currentRPC
获取当前的rpc

### Parameters
null

### Returns
an AIOU.RPC instance.

### Example
```javascript
const rpc = aiou.currentRPC();
```
