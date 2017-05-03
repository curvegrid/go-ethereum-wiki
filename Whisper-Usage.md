## Whisper API Overview

<pre><code>func (self *PublicWhisperAPI) Version() (*rpc.HexNumber, error)
</code></pre>

Returns the Whisper version this node offers.

geth console call:

<pre><code> > shh.version
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) Info() (string, error)
</code></pre>

Returns the Whisper statistics for diagnostics.

geth console call:

<pre><code> > shh.info()
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) SetMaxMessageLength(val int) error
</code></pre>

Sets the maximal message length allowed by this node.

geth console call:

<pre><code> > shh.setMaxMessageLength(999999)
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) SetMinimumPoW(val float64) error
</code></pre>

Sets the minimal PoW required by this node.

geth console call:

<pre><code> > shh.setMinimumPoW(2.12)
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) AllowP2PMessagesFromPeer(enode string) error
</code></pre>

Marks specific peer trusted, which will allow it to send historic (expired) messages.

geth console call:

<pre><code> > shh.allowP2PMessagesFromPeer("enode://d25474361659861e9e651bc728a17e807a3359ca0d344afd544ed0f11a31faecaf4d74b55db53c6670fd624f08d5c79adfc8da5dd4a11b9213db49a3b750845e@52.178.209.125:30379")

</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) HasKeyPair(id string) (bool, error)
</code></pre>

Checks if the whisper node is configured with the private key of the specified public pair.

geth console call:

<pre><code> > shh.hasKeyPair("5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) DeleteKeyPair(id string) error
</code></pre>

Deletes the specifies key if it exists.

geth console call:

<pre><code> > shh.deleteKeyPair("5e57b9ffc2387e18636e0a3d0c56b023264c16e78a2adcba1303cefc685e610f")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) NewKeyPair() (string, error)
</code></pre>

Generates a new cryptographic identity for the client, and injects it into the known identities for message decryption.

geth console call:

<pre><code> > shh.newKeyPair()
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) GetPublicKey(id string) (hexutil.Bytes, error)
</code></pre>

Returns the public key for identity id.

geth console call:

<pre><code> > shh.getPublicKey("86e658cbc6da63120b79b5eec0c67d5dcfb6865a8f983eff08932477282b77bb")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) GetPrivateKey(id string) (string, error)
</code></pre>

Returns the private key for identity id.

geth console call:

<pre><code> > shh.getPrivateKey("86e658cbc6da63120b79b5eec0c67d5dcfb6865a8f983eff08932477282b77bb")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) GenerateSymmetricKey() (string, error)
</code></pre>

Generates a random symmetric key and stores it under id, which is then returned. Will be used in the future for session key exchange.

geth console call:

<pre><code> > shh.generateSymmetricKey()
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) AddSymmetricKeyDirect(key hexutil.Bytes) (string, error)
</code></pre>

Stores the key, and returns its id.

geth console call:

<pre><code> > shh.addSymmetricKeyDirect("0xf6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) AddSymmetricKeyFromPassword(password string) (string, error)
</code></pre>

Generates the key from password, stores it, and returns its id.

geth console call:

<pre><code> > shh.addSymmetricKeyFromPassword("test")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) GetSymmetricKey(name string) (hexutil.Bytes, error)
</code></pre>

Returns the symmetric key associated with the given id.

geth console call:

<pre><code> > shh.getSymmetricKey("f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) HasSymmetricKey(id string) (bool, error)
</code></pre>

Returns true if there is a key associated with the name string. Otherwise, returns false.

geth console call:

<pre><code> > shh.hasSymmetricKey("f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) DeleteSymmetricKey(name string) (bool, error)
</code></pre>

Deletes the key associated with the name string if it exists.

geth console call:

<pre><code> > shh.deleteSymmetricKey("f6dcf21ed6a17bd78d8c4c63195ab997b3b65ea683705501eae82d32667adc92")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) GetNewSubscriptionMessages(id string) []*WhisperMessage
</code></pre>

Retrieves all the new messages matched by a filter since the last retrieval.

geth console call:

<pre><code> > shh.getNewSubscriptionMessages("02c1f5c953804acee3b68eda6c0afe3f1b4e0bec73c7445e10d45da333616412")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) GetFloatingMessages(id string) []WhisperMessage
</code></pre>

Retrieves all the floating messages that match a specific filter.
It is likely to be called once per session, right after Subscribe call.

geth console call:

<pre><code> > shh.getFloatingMessages("02c1f5c953804acee3b68eda6c0afe3f1b4e0bec73c7445e10d45da333616412")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) Subscribe(args WhisperFilterArgs) (string, error)
</code></pre>

Creates and registers a new message filter to watch for inbound whisper messages. 
Returns the ID of the newly created Filter.
Please see parameter description below.

geth console call:

<pre><code> > shh.subscribe({type: 'asym', pow: 12.3, topics: ['0x5a4ea131', '0x11223344'], key: 'b874f3bbaf031214a567485b703a025cec27d26b2c4457d6b139e56ad8734cea', sig: '0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6'})
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) Unsubscribe(id string) 
</code></pre>

Disables and removes an existing filter.

geth console call:

<pre><code> > shh.unsubscribe("02c1f5c953804acee3b68eda6c0afe3f1b4e0bec73c7445e10d45da333616412")
</code></pre>

<hr>

<pre><code>func (self *PublicWhisperAPI) Post(args PostArgs) error
</code></pre>

Creates a whisper message and injects it into the network for distribution.
Please see parameter description below.

geth console call:

<pre><code> > shh.post({type: 'asym', ttl: 7, topic: '0x07678231', powTarget: 2.01, powTime: 2, payload: '0x68656c6c6f', key: '0x048229fb947363cf13bb9f9532e124f08840cd6287ecae6b537cda2947ec2b23dbdc3a07bdf7cd2bfb288c25c4d0d0461d91c719da736a22b7bebbcf912298d1e6'})
</code></pre>

<hr>

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

type: Encryption type (symetric/asymmetric). The only valid values are "sym" or "asym".

key: ID of the decryption key (symmetric or asymmetric).

sig: Public key of the signature.

minPoW: Minimal PoW requirement for incoming messages.

topics: Array of possible topics (or partial topics).

allowP2P: Indicates if this filter allows processing of direct peer-to-peer messages (which are not to be forwarded any further, because they might be expired). This might be the case in some very rare cases, e.g. if you intend to communicate to MailServers, etc.

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
	
type: Encryption type (symetric/asymmetric). The only valid values are "sym" or "asym".

ttl: Time-to-live in seconds.

sig: ID of the signing key.

key: Key ID (in case of symmetric encryption) or public key (in case of asymmetric).

topic: Message topic (four bytes of arbitrary data).

payload: Payload to be encrypted.

padding: Optional padding (byte array of arbitrary length).

powTime: Maximal time in seconds to be spent on prrof of work.

powTarget: Minimal PoW target required for this message.

targetPeer: Optional peer ID (for peer-to-peer message only).

## Usage

Every node should treat all the messages equally, including those generated by the node itself. Therefore most users might subscribe for certain messages before sending their own. After subscription is complete, users might call GetMessages if they want to intercept the floating messages that match a the newly installed subscription filter. It might be necessary to install the encryption keys prior to subscription.

### Testing the Whisper node (geth) on private network

In order to connect to private network you need to know the enode of the bootstrap node. As of today (April 28, 2017) we have a test node with the following enode:
enode://d25474361659861e9e651bc728a17e807a3359ca0d344afd544ed0f11a31faecaf4d74b55db53c6670fd624f08d5c79adfc8da5dd4a11b9213db49a3b750845e@52.178.209.125:30379

Alternatively, you can run the diagnostic tool (wnode) as bootstrap node:

	> wnode -forwarder -standalone
	
More info on wnode tool you can find in a separate document [here](https://github.com/ethereum/go-ethereum/wiki/Whisper).

Start your geth with the following parameters:

	geth --shh --testnet --nodiscover console
	
Then connect to the bootstrap node, e.g.:
	admin.addPeer("enode://0f7f440d473c92e3734e5b93e30eb131f5a065a3673b0d191481267e777e508884ae6bd9d1aca3b995bc5044917248009877488c30f7fdd7c2f63823e4dd55dc@127.0.0.1:30379")

Now you can start playing with Whisper using geth.

### Use Cases

Below you will find several examples which illustrate the sequence of events in different scenarios. 
Each geth console command is followed by the corresponding result output, if it is relevant.

#### Receive Asymmetrically Encrypted Messages

Generate a key pair, and save its ID.

	> id = shh.newKeyPair()
	"46af9c31a30c2eeb4e6fbb5d02a0b64b62d147e576f1503372a02d4f80ebb4e1"

Retrieve and save your public key.

	> shh.getPublicKey('46af9c31a30c2eeb4e6fbb5d02a0b64b62d147e576f1503372a02d4f80ebb4e1')
	"0x048d7938066b4fb9465879c837762a767648e9473e0a6a470d719f71024d4a59450b2151b303b5f90ea35fd2e8cd91968783da17add12973e9867c626750bae3e9"

Subcribe to messages, encrypted with certain public key.
In this case we create the simplest possible subscription:

	> f = shh.subscribe({type: 'asym', key: id})
	"e6b79234d9deba9f0d963e0367fd58f7e34a13dfe9b45c3876efb1dd19f9633a"

or

	> f = shh.subscribe({type: 'asym', key: '46af9c31a30c2eeb4e6fbb5d02a0b64b62d147e576f1503372a02d4f80ebb4e1'})
	"e6b79234d9deba9f0d963e0367fd58f7e34a13dfe9b45c3876efb1dd19f9633a"

Advertise your public key.
In this case: 
0x048d7938066b4fb9465879c837762a767648e9473e0a6a470d719f71024d4a59450b2151b303b5f90ea35fd2e8cd91968783da17add12973e9867c626750bae3e9

Regulary poll for the messages, using the saved subscription ID.

	> shh.getNewSubscriptionMessages(f)

or

	> shh.getNewSubscriptionMessages('e6b79234d9deba9f0d963e0367fd58f7e34a13dfe9b45c3876efb1dd19f9633a')

result:

	[{
		hash: "0x1426abdaefe906c10d2e94a8bdb85b6626cb5e9c3c94ff36667903811836e7a1",
		padding: "0x52fdfc072182654f163f5f0f9a621d729566c74d10037c4d7bbb0407d1e2c64981855ad8681d0d86d1e91e00167939cb6694d2c422acd208a0072939487f6999eb9d18a44784045d87f3c67cf22746e995af5a25367951baa2ff6cd471c483f15fb90badb37c5821b6d95526a41a9504680b4e7c8b76",
		payload: "0x7777777777777777",
		pow: 4.4667393675027265,
		receipientPublicKey: "0x048d7938066b4fb9465879c837762a767648e9473e0a6a470d719f71024d4a59450b2151b303b5f90ea35fd2e8cd91968783da17add12973e9867c626750bae3e9",
		sig: "",
		timestamp: 1492885562,
		topic: "0x00000000",
		ttl: 7
	}]

#### Send (asymmetric encryption)

	> shh.post({type: 'asym', ttl: 7, powTarget: 2.5 powTime: 2, payload: '0x7777777777777777', key: '0x048d7938066b4fb9465879c837762a767648e9473e0a6a470d719f71024d4a59450b2151b303b5f90ea35fd2e8cd91968783da17add12973e9867c626750bae3e9'})

In this message neither Topic nor Signature is set. Payload is equivalent to an ASCII string "wwwwwwww".

#### Receive Symmetrically Encrypted Messages

In order to engage in symmetrically encrypted communication, both the parties must share the same symmetric key. In this example we assume that the parties have already exchanged the password and the Topic via a secure communication channel.

Derive symmetric key from the password, and save its ID.

	> id = shh.addSymmetricKeyFromPassword('test')
	"de6bc568f8601fac6ff2085d17c02754348ddbf4122ab1bd543a40c68d3a45fe"

Subcribe to messages, encrypted with this symmetric key.

	> f = shh.subscribe({type: 'sym', topics: ['0x07678231'], key: id})
	"07b3ab8986aa321046010f093c8ab2ba4bd441e8435f58c7c75d5398e96faf42"

or 

	> f = shh.subscribe({type: 'sym', topics: ['0x07678231'], key: 'de6bc568f8601fac6ff2085d17c02754348ddbf4122ab1bd543a40c68d3a45fe'})
	"07b3ab8986aa321046010f093c8ab2ba4bd441e8435f58c7c75d5398e96faf42"

Regulary poll for the messages, using the saved subscription ID.

	> shh.getNewSubscriptionMessages(f)

or

	> shh.getNewSubscriptionMessages('07b3ab8986aa321046010f093c8ab2ba4bd441e8435f58c7c75d5398e96faf42')

result:

	[{
		hash: "0x300b946c074e2b408b461ad685efba3686dcee90d37cdb45f975c91b2ee23489",
		padding: "0xcbe0255aa5b7d44bec40f84c892b9bffd43629b0223beea5f4f74391f445d15afd4294040374f6924b98cbf8713f8d962d7c8d019192c24224e2cafccae3a61fb586b14323a6bc8f9e7df1d929333ff993933bea6f5b3af6de0374366c4719e43a1b067d89bc7f01f1f573981659a44ff17a4c7215a3b539eb",
		payload: "0x68656c6c6f",
		pow: 6.19198790627362,
		receipientPublicKey: "",
		sig: "0x048d7938066b4fb9465879c837762a767648e9473e0a6a470d719f71024d4a59450b2151b303b5f90ea35fd2e8cd91968783da17add12973e9867c626750bae3e9",
		timestamp: 1492888296,
		topic: "0x07678231",
		ttl: 7
	}]

#### Send (symmetric encryption)

	> shh.post({type: 'sym', ttl: 7, topic: '0x07678231', powTarget: 2.01, powTime: 2, payload: '0x68656c6c6f', key: id})

or

	> shh.post({type: 'sym', ttl: 7, topic: '0x07678231', powTarget: 2.01, powTime: 2, payload: '0x68656c6c6f', key: 'de6bc568f8601fac6ff2085d17c02754348ddbf4122ab1bd543a40c68d3a45fe'})
	
If you want to sign messages you should first generate the signing key (same as asymmetric key)

	> s = shh.newKeyPair()
	"46af9c31a30c2eeb4e6fbb5d02a0b64b62d147e576f1503372a02d4f80ebb4e1"
	
and then add another parameter to the post 

	> shh.post({sig: s, type: 'sym', ttl: 7, topic: '0x07678231', powTarget: 2.01, powTime: 2, payload: '0x68656c6c6f', key: id})

#### Engage in a Chat with One-Time Session Key (for plausible deniability)

Generate symmetric key, and save its ID.

	> id = shh.generateSymmetricKey()
	"ee3ece1e35c0d3e5bd2e878dd66bf0c25b7e10df3d6b092591adca69189a6c32"

Retrieve the newly created symmetric key.

	> shh.getSymmetricKey(id)
	"3f4e735996b400637b3530d41d8bf8e0cbeafaf299aa0ad408c579569fd0af8c"

Send the raw key and Topic to you peer via a secure communication channel.
The peer should install the raw key:

	> id = shh.addSymmetricKeyDirect('0x3f4e735996b400637b3530d41d8bf8e0cbeafaf299aa0ad408c579569fd0af8c')
	"be14387971d31c6a2997dac5062978294f52a145e5a0a0a2caa4b37dbec9bb13"

Both peers (or even multiple participants) subscribe to messages, encrypted with certain key and topic.

	> f = shh.subscribe({type: 'sym', topics: ['0x07678231'],  key: id})
	"e6b79234d9deba9f0d963e0367fd58f7e34a13dfe9b45c3876efb1dd19f9633a"

Regulary poll for the messages, using the saved subscription ID.

	> shh.getNewSubscriptionMessages(f)
