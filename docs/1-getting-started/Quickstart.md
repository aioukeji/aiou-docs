---
id: Quickstart
title: Quickstart
sidebar_label: Quickstart
original_id: Quickstart
---

This page provides a guide for different kinds of users to find out which parts in the documentations to look at.

## If you want to know AIOU basic concepts

Before you get into AIOU technical details or developing smart contract on AIOU, it is helpful to know AIOU basic concepts at first and have a thorough understanding about AIOU. You can learn some basic concepts about AIOU's [account system](2-intro-of-aiou/Account.md), [economic model](2-intro-of-aiou/Economic-model.md), [vote process](2-intro-of-aiou/Vote.md).



## If you want to use AIOU

You can join AIOU community and contribute to AIOU daily operation even if you are *NOT* a developer.  
Uou can [apply for an AIOU account](https://explorer.aiou.io/applyAIOU) and [have a look at our browser](http://47.244.109.92:8006/).

## If you are a developer

For developers, there're a lot of materials to help you.

### Run and play with AIOU

You can choose to [run local single-node net](4-running-aiou-node/LocalServer.md) or [join AIOU net](4-running-aiou-node/Deployment.md).   

### Smart contracts developing

For smart contract developers, you can refer to [smart contract developing sections](3-smart-contract/ContractStart.md) for details.   

There's also som examples provided for smart contract developers, which introduce how to write/deploy/run contracts.

#### Bet Example
This example includes a bet contract writing in javascript, and a script demonstrating deploying/calling the contract from command line.

* [doc](5-lucky-bet/LuckyBet.md)
* [code](https://github.com/aiou-official/luckybet_sample)

#### Go Game Example
It is go game webpage using the AIOU blockchain backend.   

* [contract code](https://github.com/aiou-official/contracts/tree/master/demos)
* [frontend code](https://github.com/aiou-official/gobang)
* [play the game on webpage](http://47.244.109.92:8001)

There're also some contract provided by AIOU you can get use of, including [Economic Contract](6-reference/EconContract.md) and [System Contract](6-reference/SystemContract.md). You can also refer to [gas charge table](6-reference/GasChargeTable.md) to estimate contract running costs.

### SDK and API

There are also SDK and API provided for developers:

* Javascript SDK
	* [code](https://github.com/aiou-official/aiou.js)
	* Documentation on [AIOU](7-aiou-js/AIOU-class.md), [blockchain](7-aiou-js/Blockchain-class.md), [keypair](7-aiou-js/KeyPair-class.md), and [transaction](7-aiou-js/Transaction-class.md)
* Java SDK
	* [code](https://github.com/aiou-official/java-sdk)
* Python SDK
	* [code](https://github.com/aiou-official/pyost) 
* JSON RPC API
	* [Documentation](6-reference/API.md)

## If you are interested in tech details

For those of you who are interested in technical details, you can learn about [database infrastructure](2-intro-of-aiou/Database.md), [network layer](2-intro-of-aiou/Network-layer.md), and [virtual machine](2-intro-of-aiou/VM.md). You will be able to understand the internal logic of AIOU through these documentations.

## Help
Feel free to ask on [Slack](https://aiou-community.slack.com) or [AIOU forum](https://forum.aiou.io) if you have any questions.
