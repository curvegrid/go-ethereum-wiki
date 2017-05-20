# Whisper RPC API 5.0

This is the proposed API for whisper v5.

### Specs

***

#### shh_version

Returns the current semver version number.

##### Parameters

none

##### Returns

`String` - The semver version number.

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
    "memory": 10000,
    "messages": 20
  }
}
```

***

#### shh_setMaxMessageSize

Sets the maximal message size allowed by this node.
Incoming and outgoing messages with a larger size will be rejected.

##### Parameters

1. `Number`: Message size in bytes.

##### Returns

`Boolean` (`true`) on success and an error on failure.

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

#### shh_setMinimumPoW

Sets the minimal PoW required by this node.

##### Parameters

1. `val`: the new PoW requirement.

##### Returns

`Boolean` - `true` on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_setMinimumPoW","params":[12.3],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_allowP2PMessagesFromPeer

Marks specific peer trusted, which will allow it to send historic (expired) messages.

##### Parameters

1. `String`: enode of the trusted peer.

##### Returns

`Boolean` (`true`) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_allowP2PMessagesFromPeer","params":[enode://d25474361659861e9e651bc728a17e807a3359ca0d344afd544ed0f11a31faecaf4d74b55db53c6670fd624f08d5c79adfc8da5dd4a11b9213db49a3b750845e@52.178.209.125:30379],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_hasKeyPair

Checks if the whisper node is configured with the private key of the specified public pair.

##### Parameters

1. `String`: ID of key pair.

##### Returns

`Boolean` (`true` or `false`) and error on failure.

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

#### shh_deleteKeyPair

Deletes the specifies key if it exists.

##### Parameters

1. `String`: ID of key pair.

##### Returns

`Boolean` (`true`) on success and an error on failure.

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

#### shh_newKeyPair

Generates a new cryptographic identity for the client, and injects it into the known identities for message decryption.

##### Parameters

none

##### Returns

`String` (ID) on success and an error on failure.

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

#### shh_getPublicKey

Returns the public key for identity ID.

##### Parameters

1. `String`: ID of key pair.

##### Returns

`String` (public key) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getPublicKey","params":["86e658cbc6da63120b79b5eec0c67d5dcfb6865a8f983eff08932477282b77bb"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_getPrivateKey

Returns the private key for identity id.

##### Parameters

1. `String`: ID of key pair.

##### Returns

`String` (public key) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getPrivateKey","params":["86e658cbc6da63120b79b5eec0c67d5dcfb6865a8f983eff08932477282b77bb"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_addPrivateKey

Stores the key pair, and returns its ID.

##### Parameters

1. `String`: private key in hexadecimal representation.

##### Returns

`String` (ID) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_addPrivateKey","params":["8bda3abeb454847b515fa9b404cede50b1cc63cfdeddd4999d074284b4c21e15"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_generateSymmetricKey

Generates a random symmetric key and stores it under id, which is then returned. Will be used in the future for session key exchange.

##### Parameters

none

##### Returns

`String` (key ID) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_generateSymmetricKey","id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_addSymmetricKeyDirect

Stores the key, and returns its id.

##### Parameters

1. `hexutil.Bytes`: the key.

##### Returns

`String` (key ID) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_addSymmetricKeyDirect","params":["0xf6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_addSymmetricKeyFromPassword

Generates the key from password, stores it, and returns its ID.

##### Parameters

1. `String`: password.

##### Returns

`String` (key ID) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_addSymmetricKeyFromPassword","params":["test"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_getSymmetricKey

Returns the symmetric key associated with the given ID.

##### Parameters

1. `String`: key ID.

##### Returns

`hexutil.Bytes` (raw key) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getSymmetricKey","params":["f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "aeb7b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e77dd"
}
```

***

#### shh_hasSymmetricKey

Returns true if there is a key associated with the name string. Otherwise, returns false.

##### Parameters

1. `String`: key ID.

##### Returns

`Boolean` (`true` or `false`) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_hasSymmetricKey","params":["f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": "5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"
}
```

***

#### shh_deleteSymmetricKey

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

#### shh_getNewSubscriptionMessages

Retrieves all the new messages matched by a filter since the last retrieval.

##### Parameters

1. `String`: subscription ID.

##### Returns

`[]WhisperMessage` on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getNewSubscriptionMessages","params":["5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": []
}
```

***

#### shh_getFloatingMessages

Retrieves all the floating messages associated with specific subscription. It is likely to be called once per session, right after Subscribe call.

##### Parameters

1. `String`: subscription ID.

##### Returns

`[]WhisperMessage` on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getFloatingMessages","params":["02c1f5c953804acee3b68eda6c0afe3f1b4e0bec73c7445e10d45da333616412"],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": []
}
```

***

#### shh_subscribe

Creates and registers a new subscription to watch for inbound whisper messages. Returns the ID of the newly created subscription.

##### Parameters

1. `SubscribeArgs`.

##### Returns

`Boolean` - `true` on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_subscribe","params":[{type: 'asym', pow: 12.3, topics: ['0x5a4ea131', '0x11223344'], key: 'b874f3bbaf031214a567485b703a025cec27d26b2c4457d6b139e56ad8734cea', sig: '0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6'}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_unsubscribe

Disables and removes an existing subscription.

##### Parameters

1. `String`: subscription ID.

##### Returns

`Boolean` (`true` or `false`).

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

#### shh_post

Creates a whisper message and injects it into the network for distribution. 

##### Parameters

1. `PostArgs`.

##### Returns

`Boolean` (`true`) on success and an error on failure.

##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_post","params":[{type: 'asym', ttl: 7, topic: '0x07678231', powTarget: 2.01, powTime: 2, payload: '0x68656c6c6f', key: '0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6'}],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

### Parameters of Subscribe

<pre><code>func (self *PublicWhisperAPI) Subscribe(args WhisperFilterArgs) (string, error)
</code></pre>

The argument of Subscribe function is a JSON object with the following format:

		type       string
		key        string
		sig        string
		minPoW     float64
		topics     [][]byte
		allowP2P   bool

`type`: Encryption type (symetric/asymmetric). The only valid values are "sym" or "asym".

`key`: ID of the decryption key (symmetric or asymmetric).

`sig`: Public key of the signature.

`minPoW`: Minimal PoW requirement for incoming messages.

`topics`: Array of possible topics (or partial topics).

`allowP2P`: Indicates if this filter allows processing of direct peer-to-peer messages (which are not to be forwarded any further, because they might be expired). This might be the case in some very rare cases, e.g. if you intend to communicate to MailServers, etc.

### Parameters of Post

<pre><code>func (self *PublicWhisperAPI) Post(args PostArgs) error
</code></pre>

The argument of Post function is a JSON object with the following format:

	type       string
	ttl        uint32
	sig        string
	key        string
	topic      [4]byte
	padding    []byte
	payload    []byte
	powTime    uint32
	powTarget  float64
	targetPeer string
	
`type`: Encryption type (symetric/asymmetric). The only valid values are "sym" or "asym".

`ttl`: Time-to-live in seconds.

`sig`: ID of the signing key.

`key`: Key ID (in case of symmetric encryption) or public key (in case of asymmetric).

`topic`: Message topic (four bytes of arbitrary data).

`payload`: Payload to be encrypted.

`padding`: Optional padding (byte array of arbitrary length).

`powTime`: Maximal time in seconds to be spent on prrof of work.

`powTarget`: Minimal PoW target required for this message.

`targetPeer`: Optional peer ID (for peer-to-peer message only).
