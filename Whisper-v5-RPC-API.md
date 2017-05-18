# Whisper RPC API 5.0

This is the proposed API for whisper v5.

### Specs

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

#### shh_setMaxMessageLength

Sets the maximal message length allowed by this node.
This rejects incoming and outgoing messages with a size larger than set by this function.


##### Parameters

1. `Number`: Message size in bytes.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_setMaxMessageLength","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```





***

#### shh_getFloatingMessages

Retrieves all the floating messages that match a specific filter. It is likely to be called once per session, right after Subscribe call.

##### Parameters

1. `args`: WhisperFilterArgs.

##### Returns

`[]WhisperMessage` - array of Whisper messages.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_getFloatingMessages","params":[xxxxxxxx],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": [xxxxxxxxxxxxxxxxxxxxxxxxxxxx]
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```

***

#### shh_xxxxxx

xxxxxxxxxxxxxxxxxx


##### Parameters

1. `xxxxxxxxx`: xxxxxxxxxxxxxxxx.

##### Returns

`Boolean` - `true` on success and an error on failure.


##### Example
```js
// Request
curl -X POST --data '{"jsonrpc":"2.0","method":"shh_xxxxxxx","params":[234567],"id":1}'

// Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": true
}
```