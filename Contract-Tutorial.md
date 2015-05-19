# Greeter

Now that you’ve mastered the basics of Ethereum, let’s move into your first serious contract. It’s a big open territory and sometimes you might feel lonely, so our first order of business will be to create a little automatic companion to greet you whenever you feel lonely. We’ll call him the “Greeter”.

```js
contract greeter {
	function greet(bytes32 input) returns (bytes32) {
		if (input == "") {	return "Hello, World"; }
		return input; 
	}
}
```

As you can see, the Greeter is an intelligent digital entity that lives on the blockchain and is able to have conversations with anyone who interacts with it, based on it’s input. It might not be a talker, but it’s a great listener.

Before you are able to upload it to the network, you need two things: the compiled code, and the Application Binary Interface, which is a sort of user guide on how to interact with the contract.

The first you can get by using a compiler. You should have a solidity compiler built in on your geth console. To test it, use this command:

`eth.getCompilers()`

If you have it installed, it should output something like this:

`['Solidity' ]`

If instead the command returns an error, then read the documentation on how to install a compiler, use Aleth zero or use the  online solidity compiler. 

If you have Geth Solidity Compiler installed,  you need now reformat by removing spaces so it fits into a string variable:

```js
var greeterSource = 'contract greeter { function greet(bytes32 input) returns(bytes32) { if (input == "") { return "Hello, World!"; } return input; } }'
```

Once you sucessfully executed the above, compile it and publish to the network using the following commands:

```js
var greeterCompiled = eth.compile.solidity(greeterSource)
var primaryAccount = eth.accounts[0]
var greeterAddress = eth.sendTransaction({data: greeterCompiled.code, from: primaryAccount}); 
```

You will probably be asked for the password you picked in the beginning. You are choosing from which account will pay for the transaction. Wait a minute for your transaction to be picked up and then type:

```js
eth.getCode(greeterAddress)
```

This should return the code of your contract. If it returns “0x”, it means that your transaction has not been picked up yet. Wait a little bit more. If it still hasn't check if you are connected to the network

```js
net.peerCount
```

If you have more than 0 peers and it takes more than a minute or two for your transaction to be mined, your gas price might have been too low. You can experiment with different gas prices like this:

```js
var greeterAddress = eth.sendTransaction({data: greeterCompiled.code, from: primaryAccount, gas: 100000, gasPrice: web3.toWei(10, "szabo")}); 
```


The latest gas price can be checked at the [Network Stats Dashboard](https://stats.ethdev.com). Don't get hanged too much on the specific unit conventions or the values of the numbers above, just tweak around that range. Go too up and you might reach gas limit of the block, go too low and the price might be too low, or the gas insuficient for the transaction to be picked up.

After your code has been accepted, `eth.getCode(codeAddress)` will return a long string of numbers. If that’s the case, congratulations, your little Greeter is live! If the contract is created again (by performing another eth.sendTransaction), it will be published to a new address. To ensure that old contracts can be cleaned and recover it's ether balance, be sure to include a `Suicide` call on it.

Now that your contract is live on the network, anyone can interact with it by instantiating a local copy. But in order to do that, your computer needs to know how to interact with it, which is what the Application Binary Interface (ABI) is for. This is how you instantiate a contract:


```js
greeterContract = eth.contract(greeterCompiled.info.abiDefinition)
greeterInstance = new greeterContract(greeterAddress)
```

**Tip:** if the solidity compiler isn't properly installed in your machine, you can get the ABI from the online compiler . To do so, use the code below carefully replacing the first line variable with the abi from your compiler.

```js
greeterAbiDefinition = [{  constant: false,  inputs: [{    name: 'input',    type: 'bytes32'  } ],  name: 'greet',  outputs: [{    name: '',    type: 'bytes32'  } ],  type: 'function'} ]
greeterContract = eth.contract(greeterAbiDefinition)
greeterInstance = new greeterContract(greeterAddress)
```


Your instance is ready. In order to call it, just type the following command in your terminal:

```js
greeterInstance.greet.call("");
```

If your greeter returned `“Hello World”` then congratulations, you just created your first digital conversationalist bot!  Try again with: 

```
greeterInstance.greet.call("hi");
```

Try for yourself:  You can experiment changing its parameters to make it smarter. You could have it charge ether for its profound advice by adding:

```js
if (msg.value>0) { return "Thanks!"; }
```

Before the last return statement.



# NameReg

All accounts are referenced in the network by their public address. But addresses are long, difficult to write down, hard to memorize and immutable. The last one is specially important if you want to be able to generate fresh accounts in your name, or upgrade the code of your contract. In order to solve this, there is a default name registrar contract which is used to associate the long addresses with short, human-friendly names.

So let’s pick a unique name for your country. Names have to use only alphanumeric characters and, cannot contain blank spaces. In future releases the name registrar will likely implement a bidding process to prevent name squatting but for now, it's a first come first served based. So as long as no one else registered the name, you can claim it.

```js
registrar.reserve.sendTransaction("ethereumland", {from: primaryAccount});

registrar.setAddress.sendTransaction("ethereumland", eth.accounts[0], true,{from: primaryAccount});
```



Try for yourself: By typing just "registrar" you can see all the many functions available to the name registrar. Experiment with them. You can check the owner of any address by typing:

```js
registrar.owner("ethereumland")
```


# Coin

Now that you have your name secured, let's create a currency for your country. Currencies are much more interesting and useful than they seem, they are in essence just a tradeable token, but can become much more, depending on how you use them. It's value depends on it's use: a token can be used to control access (an entrance ticket), can be used for voting rights in an organization (a share), can be placeholders for an asset held by a third party (a certificate of ownership) or even be simply used as an exchange of value within a context (a currency). 

You could do all those things by creating a centralized server, but using an Ethereum token contract comes with some free qualities: for one, it's a decentralized service and tokens can be still exchanged even if the original service goes down for any reason. The code guarantees that no tokens will ever be created other than the ones set in the original code. Finally, by having each user hold it's own token, this eliminates the scenarios where one single server break in can result in the loss of funds from thousands of clients.

This is the code for the contract we're building:
 
```js
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

```js
var tokenSource = 'contract token { mapping (address => uint) balances;  function token() { balances[msg.sender] = 10000; } function sendToken(address receiver, uint amount) returns(bool sufficient) {        if (balances[msg.sender] < amount) return false;        balances[msg.sender] -= amount; balances[receiver] += amount;        return true; } function getBalance(address account) returns(uint balance){ return balances[account]; } }'
```

Now let’s set up the contract, just like we did in the previous section. Since this is a more complex contract than the Greeter, we will add more gas than the default. Extra Gas is returned.

```js
var tokenCompiled = eth.compile.solidity(tokenSource)
var primaryAccount = eth.accounts[0]
var tokenAddress = eth.sendTransaction({data: tokenCompiled.code, from: primaryAccount, gas:1000000}); 
```

Wait minute until and use the code below to test if your code has been deployed.

```js
eth.getCode(tokenAddress)
```

And then 

```js
tokenContract = eth.contract(tokenCompiled.info.abiDefinition)
tokenInstance = new tokenContract(tokenAddress)
```

You can check your own balance with:

```js
tokenInstance.getBalance.call(primaryAccount)
```

It should have all the 10 000 coins that were created once the contract was published. Since there is not any other defined way for new coins to be issued, those are all that will ever exist. 

Now of course those tokens aren't very useful if you hoard them all, so in order to send them to someone else, use this command:

```js
tokenInstance.sendToken.sendTransaction(eth.accounts[1], 100, {from: primaryAccount})
```

The reason that the first command was .call() and the second is a .sendTransaction() is that the former is just a read operation and the latter is using gas to change the state of the blockchain, and as such, it needs to be set who is it coming from. Now, wait a minute and check both accounts balances:

```js
tokenInstance.getBalance.call(eth.accounts([0])
tokenInstance.getBalance.call(eth.accounts([1])
```

**Try for yourself:** You just created your own cryptocurrency, imagine all the possibilities! Right now this cryptocurrency is quite limited as there will only ever be 10,000 coins and all are controlled by the coin creator, but you can change that. By adding the following function you issue a coin for everyone who finds an ethereum block:

```js
mapping (uint => address) miningReward;
function claimMiningReward() {
	if (msg.sender == block.coinbase && miningReward[block.number] == 0) {
		balances[msg.sender] += 1;
miningReward[block.number] = msg.sender;
	}
}
```

You could modify this to anything else: maybe reward someone who finds a solution for a new puzzle, wins a game of chess, install a solar panel—as long as that can be somehow translated to a contract. Or maybe you want to create a central bank for your personal country, so you can keep track of hours worked, favors owed or control of property. In that case you might want to add a function to allow the bank to remotely freeze funds and destroy tokens if needed.


##Future improvements, not yet implemented: 

* [Formal proofing](https://github.com/ethereum/wiki/wiki/Ethereum-Natural-Specification-Format#documentation-output) is a way where the contract developer will be able to assert some invariant qualities of the contract, like the total cap of the coin.
* [Meta coin standard](https://github.com/ethereum/cpp-ethereum/wiki/MetaCoin-API)is a proposed standardization of function names for coin and token contracts, to allow them to be automatically added to other ethereum contract that utilizes trading, like exchanges or escrow.



# Crowdfunder


Creating a country takes a lot of funds and collective effort. You could ask for donations, but donors prefer to give to projects they are more certain that will get traction and proper funding. This is an example where a crowdfunding would be ideal: you set up a goal and a deadline for reaching it. If you miss your goal, the donations are returned, therefore reducing the risk for donors. Since the code is open and auditable, there is no need for a centralized trusted platform and therefore the only fees everyone will pay are just the gas fees.

Since you already have your own internal currency, you can use that to help gather funds. In this crowdsale contract everyone who contribute will also get a proportional amount of the tokens you created. This can be used to as a proof of citizenship, as a share system or simply as a reward for their help as early pioneers.

**Attention: All contracts will be wiped out at the end of Frontier. While balances on normal addresses will be transported to Homestead, balances in contracts, as well as addresses with less than 1 ether, will not. So use this crowdfunding contract for testing purposes and don't put any significant funds unless you know what you are doing.**

```js
contract token{
function sendToken(address receiver,uint256 amount)returns(bool sufficient){}
function getBalance(address account)returns(uint256 balance){}
}

contract CrowdSale {
	
	address admin;
	address beneficiary;
    uint fundingGoal;
    uint numFunders;
    uint amount;
    uint deadline;
	uint price;
	token tokenReward;
	
    mapping (uint => Funder) funders;
	
	// data structure to hold information about campaign contributors
    struct Funder {
        address addr;
        uint amount;
    }
	

	// at initialization, setup the owner
	function CrowdSale() {
		admin = msg.sender;
	}
	
	
	function setup(address _beneficiary, uint _fundingGoal, uint _deadline, uint _price, address _reward) returns (bytes32 response){
		if (msg.sender == admin && !(beneficiary > 0 && fundingGoal > 0 && deadline > 0)) {
			beneficiary = _beneficiary;
			fundingGoal = _fundingGoal;
			deadline = _deadline;
			price = _price;
			tokenReward = token(_reward);
			
			return "campaign is set";
		} else if (msg.sender != admin) {
			return "not authorized";
		} else  {
			return "campaign cannot be changed";
		}
	}
	
    //function to contributes to the campaign
	function contribute() returns (bytes32 response) {
        Funder f = funders[numFunders++];
        f.addr = msg.sender;
        f.amount = msg.value;
        amount += f.amount;
		tokenReward.sendToken(msg.sender, f.amount/price);
		
		return "thanks for your contribution";
    }
		
    // checks if the goal or time limit has been reached and ends the campaign
    function checkGoalReached() returns (bytes32 response) {
        if (amount >= fundingGoal){
            uint i = 0; 
            beneficiary.send(amount);
	     suicide(beneficiary);
 	     return "Goal Reached!"; 
        }
        else if (deadline <= block.number){
            uint j = 0;
            uint n = numFunders;
            while (j <= n){
                funders[j].addr.send(funders[j].amount);
                funders[j].addr = 0;
                funders[j].amount = 0;
                j++;
            }
     suicide(beneficiary);
            return "Deadline passed";
        }
        return "Not reached yet";
    }
}
```

Compile it and copy the following commands on the terminal:


`> var crowdsaleCode = "0x5b33600060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908302179055505b6107c08061003b6000396000f3006000357c0100000000000000000000000000000000000000000000000000000000900480635b2329d414610045578063670c884e1461005a578063d7bb99ba1461007b57005b610050600435610409565b8060005260206000f35b6100716004356024356044356064356084356101f6565b8060005260206000f35b61008361008d565b8060005260206000f35b600060006000600860005060006003600081815054809291906001019190505581526020019081526020016000206000915091503382825060000160006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908302179055503482825060010160005081905550818150600101600050546004600082828250540192505081905550600760009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663412664ae60206000827c01000000000000000000000000000000000000000000000000000000000260005260043373ffffffffffffffffffffffffffffffffffffffff1681526020016006600050548787506001016000505404815260200160006000866161da5a03f16101c357005b5050600051507f7468616e6b7320666f7220796f757220636f6e747269627574696f6e0000000092506101f1565b505090565b6000600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff161480156102b057506000600160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1611801561029d57506000600260005054115b80156102ae57506000600560005054115b155b61036357600060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff161415610336577f63616d706169676e2063616e6e6f74206265206368616e67656400000000000090506104005661035e565b7f6e6f7420617574686f72697a65640000000000000000000000000000000000009050610400565b6103ff565b85600160006101000a81548173ffffffffffffffffffffffffffffffffffffffff0219169083021790555084600260005081905550836005600050819055508260066000508190555081600760006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908302179055507f63616d706169676e2069732073657400000000000000000000000000000000009050610400565b5b95945050505050565b6000600060006000600260005054600460005054101561057a5743600560005054111561043557610575565b6000915060036000505490505b808211151561054d576008600050600083815260200190815260200160002060005060000160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16600060086000506000858152602001908152602001600020600050600101600050546000600060006000848787f16104d257005b50505060006008600050600084815260200190815260200160002060005060000160006101000a81548173ffffffffffffffffffffffffffffffffffffffff02191690830217905550600060086000506000848152602001908152602001600020600050600101600050819055508180600101925050610442565b7f446561646c696e6520706173736564000000000000000000000000000000000093506107b8565b610790565b60009250600160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1660006004600050546000600060006000848787f16105d157005b505050600760009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663412664ae60206000827c0100000000000000000000000000000000000000000000000000000000026000526004600160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001600760009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663f8b2cb4f60206000827c01000000000000000000000000000000000000000000000000000000000260005260043073ffffffffffffffffffffffffffffffffffffffff16815260200160006000866161da5a03f161070d57005b5050600051815260200160006000866161da5a03f161072857005b505060005150600160009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16ff7f476f616c2052656163686564210000000000000000000000000000000000000093506107b8565b7f4e6f74207265616368656420796574000000000000000000000000000000000093506107b8565b50505091905056"`

`> var crowdsaleABI = [   {      "constant" : false,      "inputs" : [         {            "name" : "campaignID",            "type" : "uint256"         }      ],      "name" : "checkGoalReached",      "outputs" : [         {            "name" : "response",            "type" : "bytes32"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [         {            "name" : "_beneficiary",            "type" : "address"         },         {            "name" : "_fundingGoal",            "type" : "uint256"         },         {            "name" : "_deadline",            "type" : "uint256"         },         {            "name" : "_price",            "type" : "uint256"         },         {            "name" : "_reward",            "type" : "address"         }      ],      "name" : "setup",      "outputs" : [         {            "name" : "response",            "type" : "bytes32"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [],      "name" : "contribute",      "outputs" : [         {            "name" : "response",            "type" : "bytes32"         }      ],      "type" : "function"   }]`

Set your sending account and store the resulting contract address:
```
> var primaryAccount = eth.accounts[0]
> var crowdsaleAddress = web3.eth.sendTransaction({data: crowdsaleCode, from: primaryAccount, gas:1000000}); 
```

Wait minute until and use the code below to test if your code has been deployed.

`> eth.getCode(crowdsaleAddress)`

If it has, then do these commands to instantiate it locally.

```
crowdsaleContract = web3.eth.contract(crowdsaleABI)
crowdsaleInstance = new crowdsaleContract(crowdsaleAddress)
```

Your first step now is to set the contract up. You can only do it once and it needs to come from the same account that created the contract in the first place.

```
var beneficiary = eth.accounts[1];  // create an account for this
var fundingGoal = web3.toWei(100, "ether"); // raises a 100 ether
var deadline = eth.blockNumber + 200000; // about four weeks
var price = web3.toWei(2, "ether"); // the price of the tokens, in ether
var reward = tokenAddress; 	// the token contract address.
```

On Beneficiary put the new address that will receive the raised funds. The funding goal is the amount of ether to be raised. Deadline is measured in blocktimes which average 12 seconds, so the default is about 4 weeks. The price is tricky: but just change the number 2 for the amount of tokens the contributors will receive for each ether donated. Finally reward should be the address of the token contract you created in the last section.

`> crowdsaleInstance.setup.sendTransaction(beneficiary, fundingGoal, deadline, price, reward, {from: primaryAccount});`

Dont forget to fund your newly created contract with the necessary tokens so it can pay back the contributors!

`> tokenInstance.sendToken.sendTransaction(crowdsaleAddress, 200,{from: primaryAddress})`

You are now set. Anyone can now contribute by following these steps. First, send them the code address you just created. Now anyone can simply follow these steps:

```
var crowdsaleAddress = "0x000000"
var crowdsaleABI = [   {      "constant" : false,      "inputs" : [         {            "name" : "campaignID",            "type" : "uint256"         }      ],      "name" : "checkGoalReached",      "outputs" : [         {            "name" : "response",            "type" : "bytes32"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [         {            "name" : "_beneficiary",            "type" : "address"         },         {            "name" : "_fundingGoal",            "type" : "uint256"         },         {            "name" : "_deadline",            "type" : "uint256"         }      ],      "name" : "init",      "outputs" : [         {            "name" : "response",            "type" : "bytes32"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [],      "name" : "contribute",      "outputs" : [         {            "name" : "response",            "type" : "bytes32"         }      ],      "type" : "function"   }]
crowdsaleContract = web3.eth.contract(crowdsaleABI)
crowdsaleInstance = new crowdsaleContract(crowdsaleAddress)

var amount = web3.toWei(1, "ether") 
crowdsaleInstance.contribute.sendTransaction({from: primaryAddress, value: amount })
```

Now wait a minute for the blocks to pickup and you can check if you received the tokens or check the balance of the contract by doing this: 

`> eth.getBalance(crowdsaleAddress);`

Ethereum doesn't run contracts by itself, they have to be requested, so once the deadline is passed anyone can have the funds sent to either the beneficiary or back to the funders (if it failed) by doing a:

`> crowdsaleInstance.checkGoalReached.sendTransaction({from: primaryAddress })`

# Democracy DAO

A decentralized autonomous organization: the Democracy Contract

So you raised money for your new country, but so far it’s an Oligarchy, where all the money is controlled by the few people that have the key for your multisignature wallet. This doesn’t sound like a great start for a new society, does it? So let’s create a democratic organization.

```js
contract multisig {   
	function multisig() { 
		// when a contract has a function with the same name as itself, 
		// then that function is run at startup
		m_numOwners = 1; 
		m_required = m_numOwners; 
		m_owners[msg.sender] = m_numOwners; 
	}
    
	function transact(address _to, uint _value) external onlyowner {
		// Each transaction is converted in a hash and awaits confirmation
        if (confirm(m_owners[msg.sender], sha3(_to, _value)))
            _to.send(_value);
    }
	
	function addOwner(address _newOwner) external onlyowner {
		// Any owner can invite more, and all transactions need to be approved by more than half of them
		if (!(isOwner(_newOwner))) {
			m_numOwners++;
			m_required = m_numOwners / 2 + 1;
			m_owners[_newOwner] = m_numOwners;	
		}
	}
	
    function confirm(uint _owner, bytes32 _hash) internal returns (bool) {
		// Does some bitshifting magic to confirm transactions
        uint ownerBit = 2**_owner;
        if (m_pending[_hash].confirmed & ownerBit == 0) {
            m_pending[_hash].confirmed &= ownerBit;
            if (++m_pending[_hash].numConfirmations >= m_required) { 
                delete m_pending[_hash];
                return true;
            }
        } 
    }
	// What follows are variables that are used to describe the function
    modifier onlyowner() { if (isOwner(msg.sender)) _ }
	
	// Accessors to allow reading function variables
    function isOwner(address addr) returns (bool) { return m_owners[addr] > 0; }
    function totalOwners() returns (uint) { return m_numOwners; }
    function totalRequiredConfirmations() returns (uint) { return m_required; }
	function pendingConfirmations(address _to, uint _value) returns (uint) { return m_pending[sha3(_to, _value)].numConfirmations; }
	
	// Declaring the contract structure
    uint m_numOwners;
    uint m_required;
    mapping(address => uint) m_owners;
    mapping(bytes32 => Pending) m_pending;
    struct Pending {
        uint confirmed;
        uint numConfirmations;
    }
}
```


There’s a lot going on in this contract right now, but if you have any experience with programming languages you will probably be able to get a grasp on it. The basics is that this is a collective account with multiple account owners. Any owner can invite more people to be owners, and any owner can request money to be sent to some other account. But in order for the transaction to go through, it has to be agreed by at least 50%+1 of the account holders.

Now take that contract and compile it via the online tool provided.

The most important is the compiled hex code and the contract interface. Replace the code below by what the compiler has provided you.

`var compiledCode = "0x5b6001600060005081905550600060005054600160005081905550600060005054600260005060003373ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600050819055505b6103db806100636000396000f3006000357c010000000000000000000000000000000000000000000000000000000090048063243669ad146100665780632f54bf6e14610078578063510f43f61461008d5780637065cb481461009f578063a7cf6e22146100b0578063f77ac668146100c857005b61006e61026d565b8060005260206000f35b61008360043561022c565b8060005260206000f35b61009561027f565b8060005260206000f35b6100aa60043561019f565b60006000f35b6100be600435602435610291565b8060005260206000f35b6100d66004356024356100dc565b60006000f35b6100e53361022c565b6100ee5761019a565b610160600260005060003373ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000505460408473ffffffffffffffffffffffffffffffffffffffff166c01000000000000000000000000028152601401838152602001604090036040206102f9565b61016957610199565b8173ffffffffffffffffffffffffffffffffffffffff166000826000600060006000848787f161019557005b5050505b5b5b5050565b6101a83361022c565b6101b157610228565b6101ba8161022c565b156101c457610227565b6000600081815054809291906001019190505550600160026000600050540401600160005081905550600060005054600260005060008373ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600050819055505b5b5b50565b60006000600260005060008473ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060005054119050610268565b919050565b6000600060005054905061027c565b90565b6000600160005054905061028e565b90565b60006003600050600060408573ffffffffffffffffffffffffffffffffffffffff166c010000000000000000000000000281526014018481526020016040900360402081526020019081526020016000206000506001016000505490506102f3565b92915050565b600060008360020a905060008160036000506000868152602001908152602001600020600050600001600050541614610331576103d3565b8060036000506000858152602001908152602001600020600050600001600082828250541692505081905550600160005054600360005060008581526020019081526020016000206000506001016000818150546001019190508190551015610399576103d2565b600360005060008481526020019081526020016000206000600082016000506000905560018201600050600090555050600191506103d4565b5b5b509291505056"`

`var ABI = [   {      "constant" : false,      "inputs" : [],      "name" : "totalOwners",      "outputs" : [         {            "name" : "",            "type" : "uint256"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [         {            "name" : "addr",            "type" : "address"         }      ],      "name" : "isOwner",      "outputs" : [         {            "name" : "",            "type" : "bool"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [],      "name" : "totalRequiredConfirmations",      "outputs" : [         {            "name" : "",            "type" : "uint256"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [         {            "name" : "_newOwner",            "type" : "address"         }      ],      "name" : "addOwner",      "outputs" : [],      "type" : "function"   },   {      "constant" : false,      "inputs" : [         {            "name" : "_to",            "type" : "address"         },         {            "name" : "_value",            "type" : "uint256"         }      ],      "name" : "pendingConfirmations",      "outputs" : [         {            "name" : "",            "type" : "uint256"         }      ],      "type" : "function"   },   {      "constant" : false,      "inputs" : [         {            "name" : "_to",            "type" : "address"         },         {            "name" : "_value",            "type" : "uint256"         }      ],      "name" : "transact",      "outputs" : [],      "type" : "function"   }]`

```
var sender = eth.accounts[0]
var codeAddress = eth.sendTransaction({data: compiledCode, from: sender}); 
```

Wait minute until and use the code below to test if your code has been deployed.

`eth.getCode(codeAddress)`

If it has, then do these commands to instantiate it locally.

```
multisigContract = eth.contract(ABI)
multisigInstance = new multisigContract(codeAddress)
```

The code is ready for use. The first thing you need to do is to add a new owner to it. If you want to test it with a friend, put his address here, but if you want to test it locally, you’ll need multiple accounts. Read the section “creating a new account” above, if you haven’t done so already.

```
//.call().totalOwners()
//.call().totalRequiredConfirmations()
//.pendingConfirmations(eth.accounts[4], web3.toWei(1, "ether"))

var newSigner = eth.accounts[1]
multisigInstance.addOwner(newSigner).sendTransaction({from: sender})
```

The next step is to fund your collective account. All contracts can hold ether, just like a normal account. Notice that any ether sent to the contract belongs to it now, and will be only moved under the specific circumstances set up by its code. So before you send any significant amount, test it with a tiny bit of funds, because if anything goes wrong that money will be lost forever.

```
var amount = web3.toWei(0.01, "ether");
eth.sendTransaction({from: sender, to: codeAddress, value: amount})
```

Wait for a minute for the transaction to be picked up by the network and then you can execute this command to test the balance:

`eth.getBalance(codeAddress)`


If the balance is not zero, this means that you have successfully funded your account, and that means that your previous transaction, adding a new owner to your multi-owned account, has also gone through. We’re going to ask for your contract now to send money to someone else. Generate a third account to be the beneficiary and type the following code:

```
var beneficiary = eth.accounts[2] 
multisigInstance.sendTransaction({from: sender}).transact(beneficiary, 1000)
```

If you check the balance of the beneficiary or the contract, you’ll see that they haven’t changed, and this time it’s not a question of waiting a minute or so. It’s because for this particular contract’s transaction to go through, they need the approval of at least half, plus one, of the account owners. Since your contract is only owned by 2 accounts, it needs 2 approvals. In this case, this is done by having the second account send an identical transaction–same beneficiary and exact amount–as the previous:

`multisigInstance.sendTransaction({from: newSigner}).transact(beneficiary, 1000)`

Note: this transaction, because it’s intended to change the state of the blockchain, requires gas to be executed. If you just created the newSigner account and it doesn’t have any funds, it won’t be able to pay the gas for it to be executed. Check the section on sending transactions to learn how to send money to that account, before it can interact with the blockchain.

Now, wait a minute or so and check the balance of both the account and the beneficiary and you’ll see that the balance changed.

```
eth.getBalance(beneficiary)
eth.getBalance(codeAddress)
```

If the beneficiary now has any non zero funds means that you just created a contract with multiple owners. Think for a moment about the possibilities: you could generate the second signature in another device you own and have it as a second authentication device, such that if one of your main keys was compromised, your money would still be safe. Or you could have a friend or family use the other signature and use this contract as a joint account, or a trust fund. This could scale up to a small company, where the CFO would have to sign off with any payments coming out of the companies account. 

You just created a Decentralized Autonomous Organization. It’s an organism made of robots and people that live in the blockchain. Just like the “Greeter” bot you created earlier, once it’s public it will always exist on the blockchain, following the exact rules it was supposed to do. Unless those were explicitly stated in its constitution, there’s nothing anyone can do to embezzle it’s funds or stop it, so exercise caution before launching one into the network.

Try for yourself: Currently any owner can invite an unlimited amount of other owners. Not only there is a technical limitation because after 256 owners, then the contract will start having weird behaviors, but this is also a trust issue. You may trust someone to give a second signature, but not to invite unlimited other accounts. A single malicious user could takeover the whole account bg generating a hundred private accounts and being the only one in control of all the money. Can you fix this such issue? Maybe set a maximum number of owners, or limit the people who are able to invite more. Also see if you can change the contract to create a requirement of a minimum quorum in order for a proposal to pass.  If you can, then try to set a rule that if the transaction is above a certain value, it will require a supermajority, not a simple one. Can you figure out a way to get rid of the immigration office altogether and invite and ban members using an election?