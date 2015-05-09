# Managing accounts

**WARNING**
Remember your password. 

If you lose the password you use to encrypt your account, you will not be able to access that account.
Repeat: It is NOT possible to access your account without a password and there is no _forgot my password_ option here. Do not forget it.

The ethereum CLI `geth` provides account management via the `account` subcommand:

```
account command [arguments...]
```

Manage accounts lets you create new accounts, list all existing accounts,
import a private key into a new account.

It supports interactive mode, when you are prompted for password as well as
non-interactive mode where passwords are supplied via a given password file.
Non-interactive mode is only meant for scripted use on test networks or known
safe environments.

Make sure you remember the password you gave when creating a new account (with
either new or import). Without it you are not able to unlock your account.

Note that exporting your key in unencrypted format is NOT supported.

Keys are stored under `<DATADIR>/keys`. Make sure you backup your keys regularly! 

It is safe to transfer the entire directory or the individual keys therein between ethereum nodes.

And finally. **DO NOT FORGET YOUR PASSWORD**

```
SUBCOMMANDS:

        list    print account addresses
        new     create a new account
        import  import a private key into a new account

```

You can get info about further subcommands by `geth account help <subcommand>`

Accounts can also be managed via the [Javascript Console](https://github.com/ethereum/go-ethereum/wiki/JavaScript-Console)

## Examples
### Interactive use

```
$ geth -datadir /tmp/eth  account new
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}

$ geth --datadir /tmp/eth  account list
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}

$ geth --datadir /tmp/eth.0  account import ./key.prv
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}
```

### Non-interactive use 
You supply a plaintext password file as argument to the `--password` flag.  

**Note**: 
Supplying the password directly as part of the command line is not encouraged, but you can always use shell trickery to get round this restriction.

```
$ geth --datadir /tmp/eth --password /path/to/password account new
Address: b0047c606f3af7392e073ed13253f8f4710b08b6

$ geth --datadir /tmp/eth account list
Address: {b0047c606f3af7392e073ed13253f8f4710b08b6}

$ geth --datadir /tmp/eth1 --password /path/to/anotherpassword account import ./key.prv
Address: b0047c606f3af7392e073ed13253f8f4710b08b6
```

# Creating accounts

### Creating a new account

```
geth account new
```

Creates a new account and prints the address.

On the console, use:

```
> admin.newAccount()
```

The account is saved in encrypted format, you are prompted for a passphrase. You **must** remember this passphrase to unlock your account in the future.

For non-interactive use the passphrase can be specified with the --password flag:

```
geth --password <passwordfile> account new
```

Note, this is meant to be used for testing only, it is a bad idea to save your
password to file or expose in any other way.

### Creating an account by importing a private key

```
import [arguments...]


    geth account import <keyfile>
```

Imports an unencrypted private key from `<keyfile>` and creates a new account and prints the address.

The keyfile is assumed to contain an unencrypted private key as canonical EC
raw bytes encoded into hex.

The account is saved in encrypted format, you are prompted for a passphrase.

You must remember this passphrase to unlock your account in the future.

For non-interactive use the passphrase can be specified with the -password flag:

```
geth --password <passwordfile> account import <keyfile>
```

Note:
As you can directly copy your encrypted accounts to another ethereum instance,
this import mechanism is not needed when you transfer an account between
nodes.

**Note**:
Since you can directly copy your encrypted accounts to another ethereum instance, this import/export mechanism is not needed when you transfer an account between nodes.

**Warning:**
If you use the password flag with a password file, best to make sure the file is not readable or even listable for anyone but you. You achieve this with:

```
touch /path/to/password 
chmod 700 /path/to/password
cat > /path/to/password
>I type my pass here^D
```

# Listing accounts and checking balances

### Listing your current accounts

From the command line, call the CLI with:
```
$ geth account list

Primary #0: {d1ade25ccd3d550a7eb532ac759cac7be09c2719}
Account #1: {da65665fc30803cb1fb7e6d86691e20b1826dee0}
Account #2: {e470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32}
Account #3: {f4dd5c3794f1fd0cdc0327a83aa472609c806e99}
```

when using the console:
```
> eth.accounts
['0x407d73d8a49eeb85d32cf465507dd71d507100c1']
```

or via RPC:
```
# Request
$ curl -X POST --data '{"jsonrpc":"2.0","method":"eth_accounts","params":[],"id":1} http://127.0.0.1:8545'

# Result
{
  "id":1,
  "jsonrpc": "2.0",
  "result": ["0x407d73d8a49eeb85d32cf465507dd71d507100c1"]
}
```

### Checking account balances

To check your the etherbase account balance:
```
> web3.fromWei(eth.getBalance(eth.coinbase), "ether")
6.5
```

Print all balances with a JavaScript function:
```
function checkAllBalances() { 
var i =0; 
eth.accounts.forEach( function(e){
 	console.log("  eth.accounts["+i+"]: " +  e + " \tbalance: " + web3.fromWei(eth.getBalance(e), "ether") + " ether"); 
i++; 
})
}; 
```
That can then be executed with:
```
> checkAllBalances();
  eth.accounts[0]: 0xd1ade25ccd3d550a7eb532ac759cac7be09c2719 	balance: 63.11848 ether
  eth.accounts[1]: 0xda65665fc30803cb1fb7e6d86691e20b1826dee0 	balance: 0 ether
  eth.accounts[2]: 0xe470b1a7d2c9c5c6f03bbaa8fa20db6d404a0c32 	balance: 1 ether
  eth.accounts[3]: 0xf4dd5c3794f1fd0cdc0327a83aa472609c806e99 	balance: 6 ether
```

