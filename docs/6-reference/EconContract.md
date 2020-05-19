---
id: EconContract
title: Economic Contract
sidebar_label: Economic Contract
original_id: EconContract
---

## gas.aiou
---

GAS related contract, including pledging AIOU for gas, unpleding, transferring GAS.      
Details of economic model are introduced on [GAS economic model](2-intro-of-aiou/Economic-model.md#gas奖励)。

### Info
| contract_id | gas.aiou |
| :----: | :------ |
| language | native |
| version | 1.0.0 |

### API

#### pledge(pledgor, to, amount)
pledge aiou for gas. minimum pledging amount is 1 aiou.      
##### Example
\["user1","user1","100"\]：user1 pledge 100 aiou for himslef. So user1 costs 100 aiou, and obtains some gas.   
\["user1","user2","100"\]：user1 pledge 100 aiou for user2. So user1 costs 100 aiou, and user2 obtains some gas.

| Parameter List | Parameter Type | Remark |
| :----: | :------ |:------ |
| pledgor | string | the account who pledges aiou. **Calling this interface requires this account's "transfer" permission**|
| to | string | the account who gets gas |
| amount | string | aiou amount for pledging |

#### unpledge
undo pledge. aiou pledged earlier will be returned. minimum unpledging amount is 1 aiou.        
##### Example
\["user1","user1","100"\]：user1 unpledge 100 aiou from his earlier pledgement for himself.   
\["user1","user2","100"\]：user1 unpledge 100 aiou from his earlier pledgement for user2.

| Parameter List | Parameter Type | Remark |
| :----: | :------ |:------ |
| pledgor | string | the account who pledges aiou. **Calling this interface requires this account's "transfer" permission**|
| to | string | the account who gets gas |
| amount | string | aiou amount for pledging |


## ram.aiou
---
ram related contract, including buying/selling/transferring.    
Details of economic model are introduced on [RAM economic model](2-intro-of-aiou/Economic-model.md#resources).  
You can get price estimate in [RPC](6-reference/API.md#getraminfo) when you buy or sell a little ram. 

### Info
| contract_id | ram.aiou |
| :----: | :------ |
| language | javascript |
| version | 1.0.0 |

### API

#### buy(payer, receiver, amount)
buy RAM from system. minimum buying amount is 10 bytes.        
contract will return the amount of aiou costed.  
##### Example
\["user1","user1",1024\]:  user1 buys 1024 bytes ram for himself   
\["user1","user2",1024\]:  user1 buys 1024 bytes ram for user2   

| Parameter List | Parameter Type | Remark |
| :----: | :------ |:------ |
| payer | string | the account who buys ram. **Calling this interface requires this account's "transfer" permission** |
| receiver | string |the account who gets the bought ram|
| amount | int | ram bytes to buy |

#### sell(account, receiver, amount)
Sell unused ram to system. minimum selling amount is 10 bytes.   
contract will return the amount of aiou returned.
##### Example
\["user1","user1",1024\]:  user1 sells unused 1024 bytes ram to system, he himself gets aiou returned.
。  
\["user1","user2",1024\]:  user1 sells unused 1024 bytes ram to system, user2 gets aiou returned.

| Parameter List | Parameter Type | Remark |
| :----: | :------ |:------ |
| payer | string | the account who sells ram. **Calling this interface requires this account's "transfer" permission** |
| receiver | string |the account who gets the aiou returned|
| amount | int | ram bytes to sell |

#### lend(from, to, amount)
transfer ram to others.      
Only ram one `bought` can be transferred to others. So ram others transferred to you cannot be sold to system nor transferred to others.      
minimum transferring amount is 10 bytes.   
##### Example
\["user1","user2",1024\]: user1 transfers unused 1024 bytes ram to user2.

| Parameter List | Parameter Type | Remark |
| :----: | :------ |:------ |
| from | string | the account who transfers ram. **Calling this interface requires this account's "transfer" permission**|
| to | string | the account who receives ram |
| amount | int | ram bytes to transfer |