**WARNING**
Remember your password. 
If you lose the password you use to encrypt your account, you will not be able to access that account.
Repeat: It is NOT possible to access your account without a password and there is no forgot your password option. 
Do not forget it.

## Account management
The ethereum CLI provides account management via the `account` subcommand:

```
$ ethereum help account
account command [arguments...]

SUBCOMMANDS:
        list    print account addresses
        new     create a new account
        import  import a private key into a new account
        export  export an account into key file
```

You can get info about further subcommands by `ethereum account help <subcommand>`

### Creating a new account

```
    ethereum account new

Creates a new account.
The account is saved in encrypted format, you are prompted for a passphrase.
You must remember this passphrase to unlock your account in future.
For non-interactive use the passphrase can be specified with the --password flag:

    ethereum --password <passwordfile> account new
```

### Listing your current accounts

```
ethereum account list
```

### Creating an account by importing an EC private key (binary)

```
    ethereum account import <keyfile>

Imports a private key from <keyfile> and creates a new account with the address derived from the key.
The keyfile is assumed to contain an unencrypted private key in canonical EC format.

The account is saved in encrypted format, you are prompted for a passphrase.
You must remember this passphrase to unlock your account in future.
For non-interactive use the passphrase can be specified with the --password flag:

    ethereum --password <passwordfile> account import <keyfile>

```

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

### Exporting an account into an EC private keyfile

```
    ethereum account export <address> <keyfile>

Exports the given account's private key into keyfile using the canonical EC format.
The account needs to be unlocked, if it is not the user is prompted for a passphrase to unlock it.
For non-interactive use, the password can be specified with the --unlock flag:

    ethereum --unlock <passwrdfile> account export <address> <keyfile>

```

**Note**:
Since you can directly copy your encrypted accounts to another ethereum instance, this import/export mechanism is not needed when you transfer an account between nodes.

## Examples
### Interactive use

```
$ ethereum -datadir /tmp/eth  account new
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}

$ ethereum -datadir /tmp/eth  account list
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}

$ ethereum -datadir /tmp/eth  account export 7f444580bfef4b9bc7e14eb7fb2a029336b07c9d ./key.prv
Please enter a passphrase now.
Passphrase:

$ ethereum -datadir /tmp/eth.0  account import ./key.prv
The new account will be encrypted with a passphrase.
Please enter a passphrase now.
Passphrase:
Repeat Passphrase:
Address: {7f444580bfef4b9bc7e14eb7fb2a029336b07c9d}
```

### Non-interactive use 
You supply a plaintext password file as argument to the `-password` flag.  

**Note**: 
Supplying the password directly as part of the command line is not encouraged, but you can always use shell trickery to get round this restriction

```
$ ethereum -datadir /tmp/eth -password /path/to/password account new
Address: b0047c606f3af7392e073ed13253f8f4710b08b6

$ ethereum -datadir /tmp/eth account list
Address: {b0047c606f3af7392e073ed13253f8f4710b08b6}

$ ethereum -datadir /tmp/eth -password /path/to/password account export b0047c606f3af7392e073ed13253f8f4710b08b6 ./key.prv

$ ethereum -datadir /tmp/eth1 -password /path/to/anotherpassword account import ./key.prv
Address: b0047c606f3af7392e073ed13253f8f4710b08b6
```

### Using unencrypted file store

```
$ ethereum -datadir /tmp/eth -unencrypted-keys account new
Address: b0047c606f3af7392e073ed13253f8f4710b08b6

$ ethereum -datadir /tmp/eth -unencrypted-keys account list
Address: {b0047c606f3af7392e073ed13253f8f4710b08b6}

$ ethereum -datadir /tmp/eth -unencrypted-keys account export b0047c606f3af7392e073ed13253f8f4710b08b6 ./key.prv

$ ethereum -datadir /tmp/eth1 -unencrypted-keys account import ./key.prv
Address: b0047c606f3af7392e073ed13253f8f4710b08b6

```
