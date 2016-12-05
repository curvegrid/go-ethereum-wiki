To provide Ethereum integration for your mobile applications, the very first thing you should be interested in doing is account management.

Although all current leading Ethereum implementations provide account management built in, it is ill advised to keep accounts in any location that is shared between multiple applications and/or multiple people. The same way you do not entrust your ISP (who is after all your gateway into the internet) with your login credentials; you should not entrust an Ethereum node (who is your gateway into the Ethereum network) with your credentials either. 

The proper way to handle user accounts in your mobile applications is to do client side account management, everything self-contained within your own application. This way you can ensure as fine grained (or as coarse) access permissions to the sensitive data as deemed necessary, without relying on any third party application's functionality and/or vulnerabilities.

To support this, `go-ethereum` provides a simple, yet thorough accounts library that gives you all the tools to do properly secured account management via encrypted keystores and passphrase protected accounts. You can leverage all the security of the `go-ethereum` crypto implementation while at the same time running everything in your own application.

## Encrypted keystores

Although handling your users' accounts locally on their own mobile device does provide certain security guarantees, access keys to Ethereum accounts should never lay around in clear-text form. As such, we provide an encrypted keystore that provides the proper security guarantees for you without requiring a thorough understanding from your part of the associated cryptographic primitives.

The important thing to know when using the encrypted keystore is that the cryptographic primitives used within can operate either in *standard* or *light* mode. The former provides a higher level of security at the cost of increased computational burden and resource consumption:

 * *standard* needs 256MB memory and 1 second processing on a modern CPU to access a key
 * *light* needs 4MB memory and 100 millisecond processing on a modern CPU to access a key

As such, *light* is more suitable for mobile applications, but you should be aware of the trade-offs nonetheless.

*For those interested in the cryptographic and/or implementation details, the key-store uses the `secp256k1` elliptic curve as defined in the [Standards for Efficient Cryptography](http://www.secg.org/sec2-v2.pdf), implemented by the [`libsecp256k`](https://github.com/bitcoin-core/secp256k1) library and wrapped by [`github.com/ethereum/go-ethereum/accounts`](https://godoc.org/github.com/ethereum/go-ethereum/accounts).*

### Keystores from Android

The encrypted keystore in Android is implemented by the `AccountManager` class from the `org.ethereum.geth` package. The configuration constants (for the *standard* or *light* security modes described above) are located in the `Geth` abstract class, similarly from the `org.ethereum.geth` package. Hence to do client side account management in Android, you'll need to import two classes into your Java code:

```java
import org.ethereum.geth.AccountManager;
import org.ethereum.geth.Geth;
```

Afterwards you can create a new encrypted account manager via:

```java
try {
    AccountManager am = new AccountManager("/path/to/keystore", Geth.LightScryptN, Geth.LightScryptP);
} catch (Exception e) {
    e.printStackTrace();
}
```

The path to the keystore folder needs to be a location that is writable by the local mobile application but non-readable for other installed applications (for security reasons obviously), so we'd recommend placing it inside your app's data directory. If you are creating the `AccountManager` from within a class extending an Android object, you will most probably have access to the `Context.getFilesDir()` method via `this.getFilesDir()`, so you could set the keystore path to `this.getFilesDir() + "/keystore"`.

The last two arguments of the `AccountManager` constructor are the crypto parameters defining how resource-intensive the keystore encryption should be. You can choose between `Geth.StandardScryptN, Geth.StandardScryptP`, `Geth.LightScryptN, Geth.LightScryptP` or specify your own numbers (please make sure you understand the underlying cryptography for this). We recommend using the *light* version. 

### Keystores from iOS
