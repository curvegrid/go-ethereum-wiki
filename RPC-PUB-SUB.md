# Introduction

From version 1.4 (to be confirmed) geth has **_experimental_** support for pub/sub using subscriptions as defined in the JSON-RPC 2.0 specification. This allows clients to wait for events instead of polling for them.

It works by subscribing to particular events. The node will return a subscription id. For each event that matches the subscription a notification with relevant data is send together with the subscription id.

Example:

    // create subscription
    >> {"id": 1, "method": "eth_subscribe", "params": ["newBlocks", {}]}
    << {"jsonrpc":"2.0","id":1,"result":"0xcd0c3e8af590364c09d0fa6a1210faf5"}

    // incoming notifications
    << {"jsonrpc":"2.0","method":"eth_subscription","params":{"subscription":"0xcd0c3e8af590364c09d0fa6a1210faf5","result":{"difficulty":"0xd9263f42a87",<...>, "uncles":[]}}}
    << {"jsonrpc":"2.0","method":"eth_subscription","params":{"subscription":"0xcd0c3e8af590364c09d0fa6a1210faf5","result":{"difficulty":"0xd90b1a7ad02", <...>, "uncles":["0x80aacd1ea4c9da32efd8c2cc9ab38f8f70578fcd46a1a4ed73f82f3e0957f936"]}}}

    // cancel subscription
    >> {"id": 1, "method": "eth_unsubscribe", "params": ["0xcd0c3e8af590364c09d0fa6a1210faf5"]}
    << {"jsonrpc":"2.0","id":1,"result":true}


# Considerations
1. notifications are send for current events and not for past events. If your use case requires you not to miss any notifications than subscriptions are probably not the best option.
2. subscriptions require a full duplex connection. Geth offers such connections in the form of websockets (enable with --ws) and ipc (enabled by default).
3. subscriptions are coupled to a connection. If the connection is closed all subscriptions that are created over this connection are removed.
4. notifications are stored in an internal buffer and send from this buffer ato the client. If the client is unable to keep up and the number of buffered notifications reaches a limit (currently 10k) the connection is closed. Keep in mind that subscribing to some events can cause a flood of notifications, e.g. listening for all logs/blocks when the node starts to synchronize.
5. subscriptions for which 5 minutes no notification has been send are considered inactive and silently removed. In the future we might send a notification to the client indicating that the subscription is removed.

# Supported subscriptions
Subscriptions are creates with the `eth_subscribe` RPC method with as first param the subscription name. If the subscription was created the subscription id is returned. To remove a subscription execute the `eth_unsubcribe` RPC method with the subscription id as first param.

## newBlocks
Fires a notification each time a new block added to the chain, including chain reorganizations.

### Parameters
1. `object` with the following (optional) fields
    - **includeTransactions**, bool include transaction hashes in the notification (default false)
    - **transactionDetails**,  bool include full transaction details in the notification (default false)

### Example
    >> {"id": 1, "method": "eth_subscribe", "params": ["newBlocks", {"includeTransactions": true, "transactionDetails": false}]}
    << {"jsonrpc":"2.0","id":2,"result":"0x1106ab6329eea81560caf51f1ebff6d2"}

    << {
        "subscription":"0x1106ab6329eea81560caf51f1ebff6d2",
        "result":{
            "difficulty":"0x1010ed252a01",
            "extraData":"0xd983010305844765746887676f312e342e328777696e646f7773",
            "gasLimit":"0x3f8fc0",
            "gasUsed":"0x1ec30",
            "hash":"0x27f188384f739c9a3d063ff623046acceaab4e892f2dbb5801e602b1d429beea",
                  "logsBloom":"0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
            "miner":"0xf8b483dba2c3b7176a3da549ad41a48bb3121069",
            "nonce":"0x511e7a1ae0783aca",
            "number":"0x10ee93",
            "parentHash":"0xdd9bfdee7c618e0978797b3f93577252e27f8dffc520853b32d32ce583495ae0",
            "receiptRoot":"0x54d52af8f19adff0147e4f1cf5d92558499c9d801fb463c51dec9e08c51f2877",
            "sha3Uncles":"0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
            "size":"0x4c6",
            "stateRoot":"0x6052a897dc9607faab5092855162bdb1719f6cccc03f12a1811b6e90c655cb2e",
            "timestamp":"0x56dc83a0",
            "totalDifficulty":"0x785a9b2d1e7984f6",
            "transactions":[
                "0x70556b4fab329d6e6ed448620943903c28f5fe5bb1e7ac1cace2cf3ae51a1725",
                "0x527ad361db2dd1e501258e1810dc6f76d03fab330c52c6135c5fa7f0c023a5b6",
                "0xdf9f8895d5bc5751c63d68e14786994b51cc5820d5c621ff17487fb25b95e82a",
                "0xc87eb784b9268cf6e0557e73ea831f7ed7e23d50979d44a12c7419caeea23490",
                "0x4784a4b4fb764eaf0d9ddb2a392aee255fe75fc2d92f1660a9b03b9e3cf9ceb3",
                "0x2a76f3a67e7aae67051250b02653fb87bec537b5b44c4979e7f1b79d0cf8fbbd"
            ],
            "transactionsRoot":"0xce8215a15df64b593b4e570fafe80501d0631d428fcf2692d7d35c0a1f4db22f",
            "uncles":[]
        }
    }
    }


## logs
Returns logs that are included in new imported blocks and match the given filter criteria.

### Parameters
1. `object` with the following (optional) fields
    - **address**, either an address or an array of addresses. Only logs that are created from these addresses are returns (optional)
    - **topics**, only logs which match the specified topics (optional)


### Example
    >> {"id": 1, "method": "eth_subscribe", "params": ["logs", {"address": "0x8320fe7702b96808f7bbc0d4a888ed1468216cfd", "topics": ["0xd78a0cb8bb633d06981248b816e7bd33c2a35a6089241d099fa519e361cab902"]}]}
    << {"jsonrpc":"2.0","id":2,"result":"0x4a8a4c0517381924f9838102c5a4dcb7"}

    << {"jsonrpc":"2.0","method":"eth_subscription","params": {"subscription":"0x4a8a4c0517381924f9838102c5a4dcb7","result":[{"address":"0x8320fe7702b96808f7bbc0d4a888ed1468216cfd","blockHash":"0x61cdb2a09ab99abf791d474f20c2ea89bf8de2923a2d42bb49944c8c993cbf04","blockNumber":"0x29e87","data":"0x00000000000000000000000000000000000000000000000000000000000000010000000000000000000000000000000000000000000000000000000000000003","logIndex":"0x0","topics":["0xd78a0cb8bb633d06981248b816e7bd33c2a35a6089241d099fa519e361cab902"],"transactionHash":"0xe044554a0a55067caafd07f8020ab9f2af60bdfe337e395ecd84b4877a3d1ab4","transactionIndex":"0x0"}]}}

## newPendingTransactions
Returns all transactions that are added to the pending state and are signed with a key that is available in the node.

### Parameters
none

### Example
    >> {"id": 1, "method": "eth_subscribe", "params": ["newPendingTransactions"]}
    << {"jsonrpc":"2.0","id":2,"result":"0xc3b33aa549fb9a60e95d21862596617c"}

    << {
            "jsonrpc":"2.0",
            "method":"eth_subscription",
            "params":{
                "subscription":"0xc3b33aa549fb9a60e95d21862596617c",
                "result":"0xd6fdc5cc41a9959e922f30cb772a9aef46f4daea279307bc5f7024edc4ccd7fa"
            }
        }

## syncing
Indicates when the node starts or stops synchronizing. The result can either be a boolean indicating that the synchronization has started (true), finished (false) or an object with various indicators.

### Parameters
none

### Example
    >> {"id": 1, "method": "eth_subscribe", "params": ["sycning"]}
    << {"jsonrpc":"2.0","id":2,"result":"0xe2ffeb2703bcf602d42922385829ce96"}

    << {"subscription":"0xe2ffeb2703bcf602d42922385829ce96","result":{"syncing":true,"status":{"startingBlock":674427,"currentBlock":67400,"highestBlock":674432,"pulledStates":0,"knownStates":0}}}}