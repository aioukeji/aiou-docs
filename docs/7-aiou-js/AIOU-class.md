---
id: AIOU-class
title: AIOU
sidebar_label: AIOU
---

This is the main class that create transactions to send to AIOU blockchain and AIOU smart contracts.
## constructor
constructor method is a special method for creating and initializing AIOU class.

### Parameters
Name             |Type       |Description 
----                |--         |--
config |Object         | config object for AIOU class, config details as as follows:<br/> <b>gasRatio:</b> transaction gas price <br/> <b>gasLimit:</b> transaction gas limit <br/> <b>expiration:</b> time in seconds that transaction will be expired

### Returns
AIOU object instance.

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
call contract abi function

### Parameters
Name             |Type       |Description 
----                |--         |--
contract |String         | contract id or contract domain
abi 	 |String 		 | contract abi function name
args	 |Array			 | function args array

### Returns
Transaction Object.

### Example
```javascript
const tx = aiou.callABI(
	"token.aiou",
	"transfer",
	["aiou", "fromAccount", "toAccount", "10.000", "memo"]
);
```

## newAccount
create new AIOU blockchain account

### Parameters
Name             |Type       |Description 
----                |--         |--
name 			 |String	| new account name
creator 	 	 |String	| creator account name
ownerkey	 	 |String	| creator account ownerKey
activekey	 	 |String	| creator account activeKey
initialRAM	 	 |Number	| new account initialRAM, paid by creator
initialGasPledge |Number	| new account initialGasPledge, paid by creator

### Returns
Transaction Object.

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
transfer tokens to designated account, wrapper for callABI

### Parameters
Name             |Type       |Description 
----                |--         |--
token		|String	| token name
from 	 	|String	| transfer-from account
to			|String	| transfer-to account
amount	 	|String	| transfer amount
memo	 	|Number	| memo of this transfer

### Returns
Transaction Object.

### Example
```javascript
const tx = aiou.transfer("aiou", "fromAccount", "toAccount", "10.000", "memo");
```

# Way to send tx
Here is the way sending tx in aiou, it's compatible using third-party wallet.

```
aiou.setAccount(account);
aiou.setRPC(rpc);
aiou.signAndSend(tx)
    .on("pending", console.log)
    .on("success", console.log)
    .on("failed", console.log);
```

Third-party wallets are recommended to hook AIOU, and take over signAndSend() 
function, thus develops can work both wallet exist or not.

if wallet exists, signAndSend() works in wallet and callback when success or failed,
and if not, aiou use ```account``` to sign tx and using ```rpc``` to send it.

## signAndSend
sign a tx and send it.

### Parameters
Name             |Type       |Description 
----                |--         |--
tx       |AIOU.Tx | tx to be send

### Returns
a Callback object

### Example
```javascript
aiou.signAndSend(tx)
    .on("pending", console.log)
    .on("success", console.log)
    .on("failed", console.log);
```

## Callback.on(event, callback)
set handle to event

### Parameters
Name             |Type       |Description 
----                |--         |--
event       | String | event name, include "pending", "success", "failed"
callback       | function | callback function

### Returns
itself.

### Example
```javascript
aiou.signAndSend(tx)
    .on("pending", console.log)
    .on("success", console.log)
    .on("failed", console.log);
```


## setAccount
set default account to aiou

### Parameters
Name             |Type       |Description 
----                |--         |--
account       |AIOU.Account | account using by this aiou instance

### Returns
null

### Example
```javascript
aiou.setAccount(account);
```

## currentAccount
get current account in this aiou

### Parameters
null

### Returns
an AIOU.Account.

### Example
```javascript
const account = aiou.currentAccount();
```

## setRPC
transfer tokens to designated account, wrapper for callABI

### Parameters
Name             |Type       |Description 
----                |--         |--
rpc       |AIOU.RPC | rpc using by this AIOU instance

### Returns
null.

### Example
```javascript
aiou.setRPC(rpc);
```

## currentRPC
get current rpc

### Parameters
null

### Returns
an AIOU.RPC instance.

### Example
```javascript
const rpc = aiou.currentRPC();
```

