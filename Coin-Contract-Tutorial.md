Issue your own money: Coin contract

Now that you have your name secured, let's create a currency for your country. Currencies are much more interesting and useful than they seem, they are in essence just a tradeable token, but can become much more, depending on how you use them. It's value depends on it's use: a token can be used to control access (an entrance ticket), can be used for voting rights in an organization (a share), can be placeholders for an asset held by a third party (a certificate of ownership) or even be simply used as an exchange of value within a context (a currency). 

You could do all those things by creating a centralized server, but using an Ethereum token contract comes with some free qualities: for one, it's a decentralized service and tokens can be still exchanged even if the original service goes down for any reason. The code guarantees that no tokens will ever be created other than the ones set in the original code. Finally, by having each user hold it's own token, this eliminates the scenarios where one single server break in can result in the loss of funds from thousands of clients.
 
```
contract token { 
    mapping (address => uint) balances;
	
	// Initializes contract with 10 000 tokens to the creator of the contract
	function token() {
        balances[msg.sender] = 10000;
    }
	// Very simple trade function
    function sendToken(address receiver, uint amount) returns(bool sufficient) {
        if (balances[msg.sender] < amount) return false;
        balances[msg.sender] -= amount;
        balances[receiver] += amount;
        return true;
    }
	
	// Check balances of any account
	function getBalance(address account) returns(uint balance){
		return balances[account];
	}
	
}
```



If you have ever programmed, you won't find it hard to understand what it does: it's a contract that generates 10 thousand tokens to the creator of the contract, and then allows anyone with a balance to send it to others. 

So let's run it!
```
var tokenCompiledCode = "0x5b612710600060005060003373ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600050819055505b610172806100466000396000f3006000357c010000000000000000000000000000000000000000000000000000000090048063412664ae1461003a578063f8b2cb4f1461005257005b610048600435602435610067565b8060005260206000f35b61005d600435610134565b8060005260206000f35b600081600060005060003373ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060005054106100a4576100ad565b6000905061012e565b81600060005060003373ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008282825054039250508190555081600060005060008573ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828282505401925050819055506001905061012e565b92915050565b6000600060005060008373ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060005054905061016d565b91905056"
```
```
var tokenABI = [   {      "constant" : false,      "inputs" : [         {            "name" : "receiver",            "type" : "address"         },         {            "name" : "amount",            "type" : "uint256"         }      ],      "name" : "sendToken",      "outputs" : [         {            "name" : "sufficient",            "type" : "bool"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [         {            "name" : "account",            "type" : "address"         }      ],      "name" : "getBalance",      "outputs" : [         {            "name" : "balance",            "type" : "uint256"         }      ],      "type" : "function"   }]
```

Now let’s set up the contract, just like we did in the previous section.

```
> var primaryAddress = eth.accounts[0]
> var tokenAddress = eth.sendTransaction({data: tokenCompiledCode, from: primaryAddress, gas:150000})
```

Wait minute until and use the code below to test if your code has been deployed.


`eth.getCode(tokenAddress)`

If it has, then do these commands to instantiate it locally.

```
tokenContract = eth.contract(tokenABI)
tokenInstance = new tokenContract(tokenAddress)
```

You can check your own balance with:

`> tokenInstance.getBalance.call(primaryAddress)`

It should have all the 10 000 coins that were created once the contract was published. Since there is not any other defined way for new coins to be issued, those are all that will ever exist. 

Now of course those tokens aren't very useful if you hoard them all, so in order to send them to someone else, use this command:

`> tokenInstance.sendToken.sendTransaction(eth.accounts[1], 100, {from: primaryAddress})`

The reason that the first command was .call() and the second is a .sendTransaction() is that the former is just a read operation and the latter is using gas to change the state of the blockchain, and as such, it needs to be set who is it coming from.

**Try for yourself:** This contract has a coin that is capped to the initial issuance. Try writing a function for issuing new coins or removing coins from circulation, maybe simply depending on the owner's decisions or something more complex. Some companies might require some control of the tokens they issue, so you might need to have a function that allows assets to be frozen. 

**Learn more:**

* Formal proofing is a way where the contract developer can assert some invariant qualities of the contract, like the total cap of the coin.
* Meta coin standard is a proposed standardization of function names for coin and token contracts, to allow them to be automatically added to other ethereum contract that utilizes trading, like exchanges or escrow.
