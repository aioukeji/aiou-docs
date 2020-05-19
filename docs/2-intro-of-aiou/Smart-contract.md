---
id: Smart-contract
title: Smart Contract
sidebar_label: Smart Contract
original_id: Smart-contract
---

Smart contracts receive and execute transactions within the block, in order to maintain the variables of smart contract insides blockchain and produce irreversible proof. AIOU implements general ABI interfaces, plug-and-play multi-language support, and can generate the result of the consensus. This has substantially improved the usability of the blockchain.

## ABI Interface

AIOU smart contracts interacts with the network through ABIs.

ABIs are JSON-defined information, including the name, parameter types, etc. The supported basic types are `string`, `number`, and `bool`.

More complicated data structures can be parsed to JSON string. When calling functions in a smart contract, ABI parameter types should be strictly followed. Otherwise the execution will halt and transaction fees will incur.

```json
// example luckybet.js.abi
{
    "lang": "javascript",
    "version": "1.0.0",
    "abi": [
        {
            "name": "bet",
            "args": [
                "string",
                "number",
                "number",
                "number"
            ]
        }
    ]
}
```

Every transaction includes multiple transactional actions, and each action is a call to an ABI. All transactions will generate a strict serial on the chain, preventing double-spend attacks.

```golang
type Action struct {
	Contract   string  
	ActionName string
	Data       string  // A JSON Array of args
}
```

In a smart contract you can use `blockchain.call()` to call an ABI interface, and obtain the return value. The system will log the call stack and deny double-spend.

## Multi-language Support

AIOU achieved multi-language smart contracts. Currently, we are opening JavaScript with v8 engine, and there are native golang VM modules to handle high-performance transactions.

The smart contract engine of AIOU consists of three parts: monitor, VM, host. Monitor is the global control unit that gateways ABI calls to the right VM. VM is a virtual machine implementation of smart contracts. Host packs the runtime environments and makes sure the contracts run in the right context.

## Smart Contract Permission System

Transactions support multiple signatures. Within a contract, you can use `requireAuth()` to check if the current context bears the signature of a certain ID. Calls between smart contracts will relay signature authorizations. For example, if `A.a` calls `B.b`, authorization to `B.b` from a user is implied when `A.a` is called.

Smart contracts can check the stack of calling, and answer questions such as "Who invoked this ABI." This allows for certain operations to exist.

Smart contracts have special permissions, such as upgrading. These can be implemented with `can_update()`.

## Result of a Call

After execution, the smart contract will generate a `TxReceipt` into the block and seek consensus. You can use [RPC](6-reference/API.md#gettxreceiptbytxhash-hash) to track the TxReceipts of on-chain transactions.



