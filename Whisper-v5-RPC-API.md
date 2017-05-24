# Whisper RPC API 5.0

This is the proposed API for whisper v5.

### Specs

- [shh_version](#shh_version)
- [shh_info](#shh_info)
- [shh_setMaxMessageSize](#shh_setmaxmessagesize)
- [shh_setMinPoW](#shh_setminpow)
- [shh_markTrustedPeer](#shh_marktrustedpeer)
- [shh_newKeyPair](#shh_newkeypair)
- [shh_addPrivateKey](#shh_addprivatekey)
- [shh_deleteKeyPair](#shh_deletekeypair)
- [shh_hasKeyPair](#shh_haskeypair)
- [shh_getPublicKey](#shh_getpublickey)
- [shh_getPrivateKey](#shh_getprivatekey)
- [shh_newSymKey](#shh_newsymkey)
- [shh_addSymKey](#shh_addsymkey)
- [shh_generateSymKeyFromPassword](#shh_generatesymkeyfrompassword)
- [shh_hasSymKey](#shh_hassymkey)
- [shh_getSymKey](#shh_getsymkey)
- [shh_deleteSymKey](#shh_deletesymkey)
- [shh_subscribe](#shh_subscribe)
- [shh_unsubscribe](#shh_unsubscribe)
- [shh_pollSubscription](#shh_pollsubscription)
- [shh_getFloatingMessages](#shh_getfloatingmessages)
- [shh_post](#shh_post)


***

#### shh_version

Returns the current semver version number.

##### Parameters

none

##### Returns

`String` - The version number.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_version","params":[],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5.0.0"
}
```

***

#### shh_info

Returns diagnostic information about the whisper node.

##### Parameters

none

##### Returns

`Object` - diagnostic information with the following properties:
  - `minPow` - `Number`: current minimum PoW requirement.
  - `maxMessageSize` - `Float`: current messgae size limit in bytes.
  - `memory` - `Number`: Memory size of the floating messages in bytes.
  - `message` - `Number`: Number of floating messages.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_info","params":[],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": {
    "minPow": 12.5,
    "maxMessageSize": 20000,
    "memory": 10000,
    "messages": 20,
  }
}
```

***

#### shh_setMaxMessageSize

Sets the maximal message size allowed by this node.
Incoming and outgoing messages with a larger size will be rejected.
Whisper message size can never exceed the limit imposed by the underlying P2P protocol (10 Mb).

##### Parameters

1. `Number`: Message size in bytes.

##### Returns

`Boolean`: (`true`) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_setMaxMessageSize","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_setMinPoW

Sets the minimal PoW required by this node.

This experimental function was introduced for the future dynamic adjustment of PoW requirement. If the node is overwhelmed with messages, it should raise the PoW requirement and notify the peers. The new value should be set relative to the old value (e.g. double). The old value could be obtained via shh_info call.

**Note** This function is currently experimental.

##### Parameters

1. `Number`: The new PoW requirement.

##### Returns

`Boolean`: `true` on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_setMinPoW","params":[12.3],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_markTrustedPeer

Marks specific peer trusted, which will allow it to send historic (expired) messages.

**Note** This function is not adding new nodes, the node needs to exists as a peer.

##### Parameters

1. `String`: Enode of the trusted peer.

##### Returns

`Boolean` (`true`) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_allowTrustedPeer","params":[enode://d25474361659861e9e651bc728a17e807a3359ca0d344afd544ed0f11a31faecaf4d74b55db53c6670fd624f08d5c79adfc8da5dd4a11b9213db49a3b750845e@52.178.209.125:30379],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***


#### shh_newKeyPair

Generates a new public and private key pair for message decryption and encryption.

##### Parameters

none

##### Returns

`String`: Key ID on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_newKeyPair","id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***


#### shh_addPrivateKey

Stores the key pair, and returns its ID.

##### Parameters

1. `String`: private key as HEX bytes.

##### Returns

`String`: Key ID on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_addPrivateKey","params":["0x8bda3abeb454847b515fa9b404cede50b1cc63cfdeddd4999d074284b4c21e15"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "3e22b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_deleteKeyPair

Deletes the specifies key if it exists.

##### Parameters

1. `String`: ID of key pair.

##### Returns

`Boolean`: `true` on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_deleteKeyPair","params":["5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***


#### shh_hasKeyPair

Checks if the whisper node has a private key of a key pair matching the given ID.

##### Parameters

1. `String`: ID of key pair.

##### Returns

`Boolean`: (`true` or `false`) and error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_hasKeyPair","params":["5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": false
}
```

***

#### shh_getPublicKey

Returns the public key for identity ID.

##### Parameters

1. `String`: ID of key pair.

##### Returns

`String`: Public key on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getPublicKey","params":["86e658cbc6da63120b79b5eec0c67d5dcfb6865a8f983eff08932477282b77bb"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x3e22b9ffc2387e18636e0534534a3d0c56b023264c16e78a2adc"
}
```

***

#### shh_getPrivateKey

Returns the private key for identity ID.

##### Parameters

1. `String`: ID of the key pair.

##### Returns

`String`: Private key on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getPrivateKey","params":["86e658cbc6da63120b79b5eec0c67d5dcfb6865a8f983eff08932477282b77bb"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0x234234e22b9ffc2387e18636e0534534a3d0c56b0243567432453264c16e78a2adc"
}
```

***

#### shh_newSymKey

Generates a random symmetric key and stores it under an ID, which is then returned.
Can be used encrypting and decrypting messages where the key is known to both parties.

##### Parameters

none

##### Returns

`String`: Key ID on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_newSymKey", params: [], "id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_addSymKey

Stores the key, and returns its ID.

##### Parameters

1. `String`: The raw key for symmetric encryption as HEX bytes.

##### Returns

`String`: Key ID on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_addSymKey","params":["0xf6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_generateSymKeyFromPassword

Generates the key from password, stores it, and returns its ID.

##### Parameters

1. `String`: password.

##### Returns

`String`: Key ID on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_generateSymKeyFromPassword","params":["test"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "2e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_hasSymKey

Returns true if there is a key associated with the name string. Otherwise, returns false.

##### Parameters

1. `String`: key ID.

##### Returns

`Boolean` (`true` or `false`) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_hasSymKey","params":["f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```


***

#### shh_getSymKey

Returns the symmetric key associated with the given ID.

##### Parameters

1. `String`: key ID.

##### Returns

`String`: Raw key on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getSymKey","params":["f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "0xaeb7b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e77dd"
}
```


***

#### shh_deleteSymKey

Deletes the key associated with the name string if it exists.

##### Parameters

1. `String`: key ID.

##### Returns

`Boolean` (`true` or `false`) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_deleteSymmetricKey","params":["5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_subscribe

Creates and registers a new subscription to receive notifications for inbound whisper messages. Returns the ID of the newly created subscription.

##### Parameters

1. `id` - `String`: identifier of function call. In case of Whisper must contain the value "messages".

2. `Object`. Options object with the following properties:
  - `symKeyID` - `String`: ID of symmetric key for message decryption.
  - `privateKeyID` - `String`: ID of private (asymmetric) key for message decryption.
  - `sig` - `String`  (optional): Public key of the signature.
  - `minPow` - `Number`  (optional): Minimal PoW requirement for incoming messages.
  - `topics` - `Array`  (optional when asym key): Array of possible topics (or partial topics).
  - `allowP2P` - `Boolean`  (optional): Indicates if this filter allows processing of direct peer-to-peer messages (which are not to be forwarded any further, because they might be expired). This might be the case in some very rare cases, e.g. if you intend to communicate to MailServers, etc.

Either `symKeyID` or `privateKeyID` must be present. Can not be both.

##### Returns

`String` - The subscription ID on success, the error on failure.


##### Notification Return

`Object`: The whisper message matching the subscription options, with the following parameters:
  - `sig` - `String`: Public key who signed this message.
  - `ttl` - `Number`: Time-to-live in seconds.
  - `timestamp` - `Number`: Unix timestamp of the message genertion.
  - `topic` - `String` 4 Bytes: Message topic.
  - `payload` - `String`: Decrypted payload.
  - `padding` - `String`: Optional padding (byte array of arbitrary length).
  - `pow` - `Number`: Proof of work value.
  - `hash` - `String`: Hash of the enveloved message.


##### Example
```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_subscribe","params":[{
  topics: ['0x5a4ea131', '0x11223344'],
  symKeyID: 'b874f3bbaf031214a567485b703a025cec27d26b2c4457d6b139e56ad8734cea',
  sig: '0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6',
  pow: 12.3(?)
  }],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "02c1f5c953804acee3b68eda6c0afe3f1b4e0bec73c7445e10d45da333616412"
}


// Notification Result
{
  "jsonrpc": "2.0",
  "method": "eth_subscription",
  "params": {
    subscription: "02c1f5c953804acee3b68eda6c0afe3f1b4e0bec73c7445e10d45da333616412",
    result: {
      "sig": "0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6",
      "ttl": "0x34",
      "timestamp": "0xa34444",
      "topic": "0x5a4ea131",
      "payload": "0x3456435243142fdf1d2312",
      "padding": "0xaaa3df1d231456435243142f456435243142f2",
      "pow": "0xa",(?)
      "hash": "0xddaa3df1d231456435243142af45aa3df1d2314564352431426435243142f2",
    }
  }
}
```

***

#### shh_unsubscribe

Cancels and removes an existing subscription.

##### Parameters

1. `String`: subscription ID.

##### Returns

`Boolean`: `true` or `false`.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_unsubscribe","params":["02c1f5c953804acee3b68eda6c0afe3f1b4e0bec73c7445e10d45da333616412"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```


***

#### shh_pollSubscription (experimental) // TODO ?

Retrieves all the new messages matched by a filter since the last retrieval.

##### Parameters

1. `String`: subscription ID.

##### Returns

`Array` - containing whisper message objects on success and an error on failure. See [shh_subscribe](#shh_subscribe) for details of the object.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_pollSubscription","params":["5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": [{
      "sig": "0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6",
      "ttl": "0x34",
      "timestamp": "0xa34444",
      "topic": "0x5a4ea131",
      "payload": "0x3456435243142fdf1d2312",
      "padding": "0xaaa3df1d231456435243142f456435243142f2",
      "pow": "0xa",(?)
      "hash": "0xddaa3df1d231456435243142af45aa3df1d2314564352431426435243142f2",
  },{...}]
}
```

***

#### shh_getFloatingMessages

Retrieves all the floating messages matching the options.

##### Parameters

1. `Objects`: Same as the options parameter of [shh_subscribe](#shh_subscribe).


##### Returns

`Array` - containing whisper message objects on success and an error on failure. See [shh_subscribe](#shh_subscribe) for details of the object.


##### Example
```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getFloatingMessages","params":[{
  topics: ['0x5a4ea131', '0x11223344'],
  symKeyID: 'b874f3bbaf031214a567485b703a025cec27d26b2c4457d6b139e56ad8734cea',
  sig: '0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6',
  pow: 12.3 (?)
  }],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": [{
      "sig": "0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6",
      "ttl": "0x34",
      "timestamp": "0xa34444",
      "topic": "0x5a4ea131",
      "payload": "0x3456435243142fdf1d2312",
      "padding": "0xaaa3df1d231456435243142f456435243142f2",
      "pow": "0xa",(?)
      "hash": "0xddaa3df1d231456435243142af45aa3df1d2314564352431426435243142f2",
  },{...}]
}
```

***

#### shh_post

Creates a whisper message and injects it into the network for distribution.

##### Parameters

1. `Object`. Post options object with the following properties:
  - `symKeyID` - `String`: ID of symmetric key for message encryption.
  - `pubKey` - `String`: public key for message encryption.
  - `sig` - `String` (optional): ID of the signing key.
  - `ttl` - `Number`: Time-to-live in seconds.
  - `topic` - `String` 4 Bytes (optional when key is asym): Message topic.
  - `payload` - `String`: Payload to be encrypted.
  - `padding` - `String` (optional): Optional padding (byte array of arbitrary length).
  - `powTime` - `Number`: Maximal time in seconds to be spent on proof of work.
  - `powTarget` - `Number`: Minimal PoW target required for this message.
  - `targetPeer` - `String` (optional): Optional peer ID (for peer-to-peer message only).

Either `symKeyID` or `pubKey` must be present. Can not be both.

##### Returns

`Boolean`: `true` on success and an error on failure.

##### Example
```
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_post","params":[{
  pubKey: 'b874f3bbaf031214a567485b703a025cec27d26b2c4457d6b139e56ad8734cea',
  ttl: 7,
  topic: '0x07678231',
  powTarget: 2.01,
  powTime: 2,
  payload: '0x68656c6c6f'
  }],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```
