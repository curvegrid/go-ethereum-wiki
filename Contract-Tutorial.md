# Registrars

## NameReg

### Easier addresses: the Name Registrar

All accounts are referenced in the network by their public address. But addresses are long, difficult to write down, hard to memorize and immutable. The last one is specially important if you want to be able to generate fresh accounts in your name, or upgrade the code of your contract. In order to solve this, there is a default name registrar contract which is used to associate the long addresses with short, human-friendly names.

Names have to use only alphanumeric characters and, cannot contain blank spaces. In future releases the name registrar will likely implement a bidding process to prevent name squatting but for now, it's a first come first served based. So as long as no one else registered the name, you can claim it.

First, select your name:

    var myName = "bob"

Then, check the availability of your name:

    registrar.addr(myName)

If that function returns "0x00..", you can claim it to yourself:

    registrar.reserve.sendTransaction(myName, {from: eth.accounts[0]});

Wait for the previous transaction to be picked up. Wait up to thirty seconds and then try:

    registrar.owner(myName)

 If it returns your address, it means you own that name and are able to set your chosen name to any address you want:

    registrar.setAddress.sendTransaction(myName, eth.accounts[1], true,{from: eth.accounts[0]});


You can send a transaction to anyone by name instead of account simply by typing 

    eth.sendTransaction({from: eth.accounts[0], to: registrar.addr("bob"), value: web3.toWei(1, "ether")})


**Tip: don't mistake registrar.addr for registrar.owner. The first is to which address that name is pointed at: anyone can point a name to anywhere else, just like anyone can forward a link to google.com, but only the owner of the name can change and update the link. You can set both to be the same address.**


# Greeter
Now that you mastered the basics on how to get started and how to send ether, it's time to get your hands dirty in what really makes ethereum stand out of the crowd: smart contracts. Smart contracts are pieces of code that live on the blockchain and execute commands exactly how they were told to. They can read other contracts, take decisions, send ether and execute other contracts. Contracts will exist and run as long as the whole network exists, and will only stop if they run out of gas or if they were programmed to self destruct.

What can you do with contracts? You can do almost anything really, but for this guide let's do something simple: you will start your new country.

Your country won't be very powerful compared to most: it will hold no land, have no military and hold no assets other than those that exist on the blockchain. All it's citizens will be voluntary and it is unable to coerce other people by force. 

But what it can do is to gather support around a united cause. You will get funds through a crowdfunding that, if successful, will supply a radically transparent and democratic organization that will only obey its own citizens, will never swerve away from its constitution and cannot be censored or shut down. And all that in less than 300 lines of code.

So let's start now.

**Important: Frontier is considered a test network. All contracts might be wiped when the project transitions to the next phase, and all ether they contain will be lost. Only send small amounts of funds to contracts, unless are okay losing them.**

## Your first citizen: the greeter

[Full documentation](https://github.com/ethereum/go-ethereum/wiki/Contracts-and-Transactions)


Now that you’ve mastered the basics of Ethereum, let’s move into your first serious contract. It’s a big open territory and sometimes you might feel lonely, so our first order of business will be to create a little automatic companion to greet you whenever you feel lonely. We’ll call him the “Greeter”.


    contract greeter {
        // Declare variable admin which will store an address
        address public admin;

        // this function is executed at initialization 
        // and sets the owner of the contract
        function greeter() {
            admin = msg.sender;
        }

        // main function
        function greet(bytes32 input) returns (bytes32) {
            if (input == "") {  return "Hello, World"; }
            return input; 
        }
       
        // Function to recover the funds on the contract
        function kill() {
            if (msg.sender == admin) {
                suicide(admin);
            }
        }
    }



As you can see, the Greeter is an intelligent digital entity that lives on the blockchain and is able to have conversations with anyone who interacts with it, based on its input. It might not be a talker, but it’s a great listener.

Before you are able to upload it to the network, you need two things: the compiled code, and the Application Binary Interface, which is a sort of user guide on how to interact with the contract.

The first you can get by using a compiler. You should have a solidity compiler built in on your geth console. To test it, use this command:

    eth.getCompilers()

If you have it installed, it should output something like this:

    ['Solidity' ]

If instead the command returns an error, then read the documentation on how to install a compiler, use Aleth zero or use the  online solidity compiler. 

If you have Geth Solidity Compiler installed,  you need now reformat by removing spaces so it fits into a string variable (there are some online tools that will do this):

    var greeterSource = 'contract greeter { address admin; function greeter() { admin = msg.sender; } function greet(bytes32 input) returns (bytes32) { if (input == "") { return "Hello, World"; } return input; } function kill() { if (msg.sender == admin) { suicide(admin); } }}'



Once you successfully executed the above, compile it and publish to the network using the following commands:

    var greeterCompiled = eth.compile.solidity(greeterSource)
    var greeterAddress = eth.sendTransaction({data: greeterCompiled.greeter.code, from: eth.accounts[0], gas:1000000, gasPrice: web3.toWei(0.001, "finney")}); 

You will probably be asked for the password you picked in the beginning. You are choosing from which account will pay for the transaction. Depending on the current gas price, expect that this contract to cost approximately 0.5 ether.

You can take a look to see if your transaction is on the list of pending transactions waiting to be picked up:

    eth.pendingTransactions()

Wait a minute for your transaction to be picked up and then type:

    eth.getCode(greeterAddress)

This should return the code of your contract. If it returns “0x”, it means that your transaction has not been picked up yet. Wait a little bit more. If it still hasn't, check if you are connected to the network

    net.peerCount

If you have more than 0 peers and it takes more than a minute or two for your transaction to be mined, your gas price might have been too low. Go back to that command above and try playing around with the gas price or gas amount. Go too up and you might reach gas limit of the block, go too low and the price might be too low, or the gas insufficient for the transaction to be picked up.

After your code has been accepted, eth.getCode(codeAddress) will return a long string of numbers. If that’s the case, congratulations, your little Greeter is live! If the contract is created again (by performing another eth.sendTransaction), it will be published to a new address. 

Now that your contract is live on the network, anyone can interact with it by instantiating a local copy. But in order to do that, your computer needs to know how to interact with it, which is what the Application Binary Interface (ABI) is for. To generate a contract from ABI you have to do this:

    greeterContract = eth.contract(greeterCompiled.greeter.info.abiDefinition)

**Tip: if the solidity compiler isn't properly installed in your machine, you can get the ABI from the online compiler . To do so, use the code below carefully replacing greeterCompiled.greeter.info.abiDefinition  with the abi from your compiler.**

After having created a local copy of the object, this is how you actually instantiate that object from a live contract address:

    greeterInstance = greeterContract.at(greeterAddress)

Alternatively, those two lines could be written together in a single call:

    greeterInstance = eth.contract(greeterCompiled.greeter.info.abiDefinition).at(greeterAddress)


Your instance is ready. In order to call it, just type the following command in your terminal:

    greeterInstance.greet.call("");

If your greeter returned “Hello World” then congratulations, you just created your first digital conversationalist bot!  Try again with: 

    greeterInstance.greet.call("hi");


### Cleaning up after yourself: 

You must be very excited to have your first contract live, but this excitement wears off sometimes, when the owners go on to write further contracts, leading to the unpleasant sight of abandoned contracts on the blockchain. In the future, blockchain rent might be implemented in order to increase the scalability of the blockchain but for now, be a good citizen and humanely put down your abandoned bots. The suicide is subsidized by the contract creation so it will cost much less than a usual transaction.

    greeterInstance.kill.sendTransaction({from:eth.accounts[0], gasPrice: web3.toWei(0.001, "finney")})

Notice that every contract has to implement it's own kill clause. In this particular case only the account that created the contract can kill it. If you don't add any kill clause it could potentially live forever (or at least until the frontier contracts are all wiped) independently of you and any earthly borders, so before you put it live check what your local laws say about it, including any possible limitation on technology export, restrictions on speech and maybe future legislation on civil rights of sentient digital beings.


**Try it yourself:  You can experiment changing its parameters to make it smarter. Challenge yourself to have it charge ether for its profound advice by adding the following function on the "greet". Here's a simple example on how to make the greeter into a joker by making it sell jokes\*:**


    if (input=="Who's there?") { 
    /* insert a joke here */
    } else if (msg.value > 1000) { 
    /* a trillionth of an ether. It's a cheap joke. */
    return "Knock knock!"; 
    }

Any balance your greeter is able to make will be forwarded to you on the kill call. 

_\* Actually the blockchain is open source and anyone could read your joke for free, but has anyone ever laughed by reading source code?_

*Test it with others: anyone else running the geth console can interact with your contract by first instantiating it using this line of code:*

    greeterInstance = eth.contract([{constant:false,inputs:[],name:'kill',outputs:[],type:'function'},{constant:false,inputs:[{name:'input',type:'bytes32'}],name:'greet',outputs:[{name:'',type:'bytes32'}],type:'function'},{inputs:[],type:'constructor'} ]).at(greeterAddress);


# Coin


Now let's create a coin for your country. Coins are much more interesting and useful than they seem, they are in essence just a tradeable token, but can become much more, depending on how you use them. It's value depends on it's use: a token can be used to control access (an entrance ticket), can be used for voting rights in an organization (a share), can be placeholders for an asset held by a third party (a certificate of ownership) or even be simply used as an exchange of value within a community (a currency). 

You could do all those things by creating a centralized server, but using an Ethereum token contract comes with some free qualities: for one, it's a decentralized service and tokens can be still exchanged even if the original service goes down for any reason. The code guarantees that no tokens will ever be created other than the ones set in the original code. Finally, by having each user hold it's own token, this eliminates the scenarios where one single server break in can result in the loss of funds from thousands of clients.

This is the code for the contract we're building:
 

    contract token { 
        mapping (address => uint) public coinBalanceOf;
      
      /* Initializes contract with 10 000 tokens to the creator of the contract */
      function token() {
            coinBalanceOf[msg.sender] = 10000;
        }
      
      /* Very simple trade function */
        function sendCoin(address receiver, uint amount) returns(bool sufficient) {
            if (coinBalanceOf[msg.sender] < amount) return false;
            coinBalanceOf[msg.sender] -= amount;
            coinBalanceOf[receiver] += amount;
            return true;
        }

    }


If you have ever programmed, you won't find it hard to understand what it does: it's a contract that generates 10 thousand tokens to the creator of the contract, and then allows anyone with a balance to send it to others. These tokens are the minimum tradeable unit and cannot be subdivided, but for the final users could be presented as a 100 units subdividable by 100 subunits, so owning a single token would represent having 0.01% of the total. If your application needs more fine grain atomic divisibility, then just increase the initial issuance amount.

In this example we declared the variable "balance" to be public, this will automatically create a function that checks any accounts balance.

**So let's run it!**

    var tokenSource = 'contract token { mapping (address => uint) public balances; /* Initializes contract with 10 000 tokens to the creator of the contract */ function token() { balances[msg.sender] = 10000; } /* Very simple trade function */ function sendToken(address receiver, uint amount) returns(bool sufficient) { if (balances[msg.sender] < amount) return false; balances[msg.sender] -= amount; balances[receiver] += amount; return true; } }'

Now let’s set up the contract, just like we did in the previous section..

    var tokenCompiled = eth.compile.solidity(tokenSource)
    var tokenAddress = eth.sendTransaction({data: tokenCompiled.token.code, from: eth.accounts[0], gas:1000000, gasPrice: web3.toWei(0.001, "finney")}); 

Wait minute until and use the code below to test if your code has been deployed.

    eth.pendingTransactions();

    eth.getCode(tokenAddress)

And then 

    tokenInstance = eth.contract(tokenCompiled.token.info.abiDefinition).at(tokenAddress)


You can check your own balance with:

    tokenInstance.balances.call(eth.accounts[0])

It should have all the 10 000 tokens that were created once the contract was published. Since there is not any other defined way for new coins to be issued, those are all that will ever exist. 

Now of course those tokens aren't very useful if you hoard them all, so in order to send them to someone else, use this command:

    tokenInstance.sendToken.sendTransaction(eth.accounts[1], 1000, {from: eth.accounts[0]})

If a friend has registered a name on the registrar you can send it without knowing their address, doing this:

    tokenInstance.sendToken.sendTransaction(registrar.addr("Alice"), 2000, {from: eth.accounts[0]})


The reason that the first command was .call() and the second is a .sendTransaction() is that the former is just a read operation and the latter is using gas to change the state of the blockchain, and as such, it needs to be set who is it coming from. Now, wait a minute and check both accounts balances:

    tokenInstance.balances.call(eth.accounts[0])
    tokenInstance.balances.call(eth.accounts[1])
    tokenInstance.balances.call(registrar.addr("Alice"))

**Try for yourself: You just created your own cryptocurrency! Right now this cryptocurrency is quite limited as there will only ever be 10,000 coins and all are controlled by the coin creator, but you can change that. You could for example reward ethereum miners, by creating a transaction that will reward who found the current block:**

    mapping (uint => address) miningReward;
    function claimMiningReward() {
      if (miningReward[block.number] == 0) {
        balances[block.coinbase] += 1;
    miningReward[block.number] = block.coinbase;
      }
    }

You could modify this to anything else: maybe reward someone who finds a solution for a new puzzle, wins a game of chess, install a solar panel—as long as that can be somehow translated to a contract. Or maybe you want to create a central bank for your personal country, so you can keep track of hours worked, favors owed or control of property. In that case you might want to add a function to allow the bank to remotely freeze funds and destroy tokens if needed.


### Getting others to interact with your contract

The commands mentioned only work because you have tokenInstance instantiated on your local machine. If you send tokens to someone they won't be able to move them forward because they don't have the same object. In fact if you restart your console these objects will be deleted and the contracts you've been working on will be lost forever. So how do you instantiate the contract on a clean machine? 

There are two ways. Let's start with the quick and dirty, providing your friends with a reference to your contract’s ABI:

    tokenInstance = eth.contract([{constant:false,inputs:[{name:'receiver',type:'address'},{name:'amount',type:'uint256'}],name:'sendToken',outputs:[{name:'sufficient',type:'bool'}],type:'function'},{type:'function',constant:true,inputs:[{name:'',type:'address'}],name:'balance',outputs:[{name:'',type:'uint256'}]},{inputs:[],type:'constructor'}]).at('0x4a4ce7844735c4b6fc66392b200ab6fe007cfca8')

Just replace the address at the end for your own token address, then anyone that uses this snippet will immediately be able to use your contract. Of course this will work only for this specific contract so let's analyze step by step and see how to improve this code so you'll be able to use it anywhere.

First, if you register a name, then you won't need the hard coded address in the end. Select a nice coin name for you and try to reserve for yourself.

    var tokenName = "MyFirstCoin"
    registrar.reserve.sendTransaction(tokenName, {from: eth.accounts[0]});

Wait for the previous transaction to be picked up and then set that name to point to your coin address:

    registrar.setAddress.sendTransaction(tokenName, tokenAddress, true,{from: eth.accounts[0]});

Wait a little bit for that transaction to be picked up too and test it:

    registrar.addr("MyFirstCoin")

This should now return your token address, meaning that now the previous code to instantiate could use a name instead of an address.

    tokenInstance = eth.contract([{constant:false,inputs:[{name:'receiver',type:'address'},{name:'amount',type:'uint256'}],name:'sendToken',outputs:[{name:'sufficient',type:'bool'}],type:'function'},{type:'function',constant:true,inputs:[{name:'',type:'address'}],name:'balance',outputs:[{name:'',type:'uint256'}]},{inputs:[],type:'constructor'}]).at(registrar.addr("MyFirstCoin"))

This also means that the owner of the coin can update the coin by pointing the registrar to the new contract. This would, of course, require the coin holders trust the owner set at  registrar.owner("MyFirstCoin")

The code is still long and not very friendly, and that's mostly because of the ABI that uses most of the contract code space. Ideally the only thing the user should need to know to access the contract would be it's name. In order to do that we have to register the abi somewhere also, which what the Contract Metadata Registry is for.

To  initiate the process, execute this:

    admin.contractInfo.newRegistry(eth.accounts[0])

In the future Ethereum will have support for a pure hash-based content system to allow any data to be saved in the P2P network, but for now we'll have to create some files and upload them manually. First create a file on your system (e.g. in your desktop, if you're messy like me) and add its path like this:

    var localFilePath = '/Users/yourusername/Desktop/abi.json'

Now use this line to write to that file and save its hash:

    contentHash = admin.contractInfo.register(eth.accounts[0], tokenAddress, tokenCompiled.token, localFilePath)

You now have to put the .json file you just created somewhere publicly accessible. If you have an http server you can just drag into it, but you can also use a service like PasteBin or Gist. Create a new unlisted/private file, copy the content from the file you just created and save it. Then click the "raw" link and get the link like this:

    var remoteFilePath = 'https://gist.githubusercontent.com/alexvandesande/ee0d34f5ac47937b6330/raw/6813c4e5eee9af33a1728565141ca4572530ffcd/abi.json'

Now finally you can register the ABI with:

    admin.contractInfo.registerUrl(eth.accounts[0], contentHash, remoteFilePath)

Wait for the miners to pick it up and check if everything went well with:

    admin.contractInfo.get(registrar.addr(tokenName)).AbiDefinition

This should return the ABI. If it doesn't, then double check the process and maybe try uploading your file to a different host. Now in order to instantiate the contract in any computer all one has to know is either its address or registered name.

    var tokenAddress = registrar.addr("MyFirstCoin")
    var tokenInstance = eth.contract(admin.contractInfo.get(tokenAddress).AbiDefinition).at(tokenAddress)


### Learn More 

* [Meta coin standard](https://github.com/ethereum/wiki/wiki/Standardized_Contract_APIs) is a proposed standardization of function names for coin and token contracts, to allow them to be automatically added to other ethereum contract that utilizes trading, like exchanges or escrow.

* [Formal proofing](https://github.com/ethereum/wiki/wiki/Ethereum-Natural-Specification-Format#documentation-output) is a way where the contract developer will be able to assert some invariant qualities of the contract, like the total cap of the coin. *Not yet implemented*.



# Crowdfunder

Creating an organization takes a lot of funds and collective effort. You could ask for donations, but donors prefer to give to projects they are more certain that will get traction and proper funding. This is an example where a crowdfunding would be ideal: you set up a goal and a deadline for reaching it. If you miss your goal, the donations are returned, therefore reducing the risk for donors. Since the code is open and auditable, there is no need for a centralized trusted platform and therefore the only fees everyone will pay are just the gas fees.

Since you already have your own internal currency, you can use that to help gather funds. In this crowdsale contract everyone who contribute will also get a proportional amount of the tokens you created. This can be used to as a proof of citizenship, as a share system or simply as a reward for their help as early pioneers.

**Attention: All contracts could be wiped out at the end of Frontier. While balances on normal addresses will be transported to Homestead, balances in contracts, as well as addresses with less than 1 ether, will not. So use this crowdfunding contract for testing purposes and don't put any significant funds unless you know what you are doing.**

### The code

    contract token { mapping (address => uint) public balance; function token() {}  function sendToken(address receiver, uint amount) returns(bool sufficient) {  } }

    contract CrowdSale {
        
        address public admin;
        address public beneficiary;
        uint public fundingGoal;
        uint public numFunders;
        uint public amountRaised;
        uint public deadline;
        uint public price;
        token public tokenReward;   
        mapping (uint => Funder) public funders;
        
        /* data structure to hold information about campaign contributors */
        struct Funder {
            address addr;
            uint amount;
        }
        
        /*  at initialization, setup the owner */
        function CrowdSale() {
        admin = msg.sender;
        }   
        
        function setup(address _beneficiary, uint _fundingGoal, uint _duration, uint _price, address _reward) returns (bytes32 response){
            if (msg.sender == admin && !(beneficiary > 0 && fundingGoal > 0 && deadline > 0)) {
                beneficiary = _beneficiary;
                fundingGoal = _fundingGoal;
                deadline = now + _duration * 1 days;
                price = _price;
                tokenReward = token(_reward);
                
                return "campaign is set";
            } else if (msg.sender != admin) {
                return "not authorized";
            } else  {
                return "campaign cannot be changed";
            }
        }
        
        /* The function without name is the default function that is called whenever anyone sends funds to a contract */
        function () returns (bytes32 response) {
            Funder f = funders[numFunders++];
            f.addr = msg.sender;
            f.amount = msg.value;
            amountRaised += f.amount;
            tokenReward.sendToken(msg.sender, f.amount/price);
            
            return "thanks for your contribution";
        }
            
        /* checks if the goal or time limit has been reached and ends the campaign */
        function checkGoalReached() returns (bytes32 response) {
            if (amountRaised >= fundingGoal){
                uint i = 0; 
                beneficiary.send(amountRaised);
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

You know the drill. Remove line breaks and copy the following commands on the terminal:


    var crowdsaleSource = 'contract token { mapping (address => uint) public balance; function token() {}  function sendToken(address receiver, uint amount) returns(bool sufficient) {  } } contract CrowdSale { address public admin; address public beneficiary; uint public fundingGoal; uint public numFunders; uint public amountRaised; uint public deadline; uint public price; token public tokenReward; mapping (uint => Funder) public funders; /* data structure to hold information about campaign contributors */ struct Funder { address addr; uint amount; } /* at initialization, setup the owner */ function CrowdSale() { admin = msg.sender; } function setup(address _beneficiary, uint _fundingGoal, uint _duration, uint _price, address _reward) returns (bytes32 response){ if (msg.sender == admin && !(beneficiary > 0 && fundingGoal > 0 && deadline > 0)) { beneficiary = _beneficiary; fundingGoal = _fundingGoal; deadline = now + _duration * 1 days; price = _price; tokenReward = token(_reward); return "campaign is set"; } else if (msg.sender != admin) { return "not authorized"; } else { return "campaign cannot be changed"; } } /* The function without name is the default function that is called whenever anyone sends funds to a cntract */ function () returns (bytes32 response) { Funder f = funders[numFunders++]; f.addr = msg.sender; f.amount = msg.value; amountRaised += f.amount; tokenReward.sendToken(msg.sender, f.amount/price); return "thanks for your contribution"; } /* checks if the goal or time limit has been reached and ends the campaign */ function checkGoalReached() returns (bytes32 response) { if (amountRaised >= fundingGoal){ uint i = 0; beneficiary.send(amountRaised); suicide(beneficiary); return "Goal Reached!"; } else if (deadline <= block.number){ uint j = 0; uint n = numFunders; while (j <= n){ funders[j].addr.send(funders[j].amount); funders[j].addr = 0; funders[j].amount = 0; j++; } suicide(beneficiary); return "Deadline passed"; } return "Not reached yet"; } }'

    var crowdsaleCompiled = eth.compile.solidity(crowdsaleSource);
    var crowdsaleAddress = eth.sendTransaction({data: crowdsaleCompiled.CrowdSale.code, from: eth.accounts[0], gas:1000000, gasPrice: web3.toWei(0.001,"finney")}); 

Wait minute until and use the code below to test if your code has been deployed.

    eth.pendingTransactions();
    eth.getCode(crowdsaleAddress)


If it has, then do these commands to instantiate it locally.


    crowdsaleInstance = web3.eth.contract(crowdsaleCompiled.CrowdSale.info.abiDefinition).at(crowdsaleAddress)


### Set Up 

Your first step now is to set the contract up. You can only do it once and it needs to come from the same account that created the contract in the first place.

    var beneficiary = eth.accounts[1];    // create an account for this
    var fundingGoal = web3.toWei(100, "ether"); // raises a 100 ether
    var duration = 7;     // number of days the campaign will last
    var pri
    ce = web3.toWei(0.02, "ether"); // the price of the tokens, in ether
    var reward = registrar.addr("MyFirstCoin");   // the token contract address.

On Beneficiary put the new address that will receive the raised funds. The funding goal is the amount of ether to be raised. Deadline is measured in blocktimes which average 12 seconds, so the default is about 4 weeks. The price is tricky: but just change the number 2 for the amount of tokens the contributors will receive for each ether donated. Finally reward should be the address of the token contract you created in the last section.

In this example you are sending to the crowdsale fund 50% of all the tokens that ever existed, in exchange for 100 ether. Decide those parameters very carefully as they will play a very important role on the next part of our guide.

    crowdsaleInstance.setup.sendTransaction(beneficiary, fundingGoal, deadline, price, reward, {from: eth.accounts[0], gas: 150000, gasPrice:web3.toWei(0.001, "finney")});

Dont forget to fund your newly created contract with the necessary tokens so it can pay back the contributors!

    tokenInstance.sendToken.sendTransaction(crowdsaleAddress, 5000,{from: eth.accounts[0]})

After the transaction is picked, you can check the amount of tokens the crowdsale address has, and all other variables this way:

    tokenInstance.balance.call(crowdsaleAddress)
    crowdsaleInstance.beneficiary.call()
    crowdsaleInstance.amountRaised.call()
    crowdsaleInstance.fundingGoal.call()


### Register a name

You are now set. Anyone can now contribute by simply sending ether to the crowdsale address, but to make it even simpler, let's register a name for your sale. First, pick a name for your crowdsale:

    var name = "mycrowdsale"

Check if that's available and register:

    registrar.addr(name) 
    registrar.reserve.sendTransaction(name, {from: eth.accounts[0]});
 
Wait for the previous transaction to be picked up and then:

    registrar.setAddress.sendTransaction(name, crowdsaleAddress, true,{from: eth.accounts[0]});

Now anyone can contribute to it by simply executing this command: 

    var amount = web3.toWei(4, "ether") // decide how much to contribute

    eth.sendTransaction({from: eth.accounts[0], to: registrar.addr("mycrowdsale"), value: amount, gas:1000000, gasPrice:web3.toWei(0.001, "finney") })

Now wait a minute for the blocks to pickup and you can check if the contract received the ether by doing this: 

    eth.getBalance(crowdsaleAddress);

If the balance has changed, use now this to check if you received tokens

    tokenInstance.balance.call(eth.accounts[0])


This allows anyone to simply send ether to the contract, but others might want to interact more deeply with it, for example they may want to build a service that regularly checks the progress of the crowdfund, or to inspect who are it's funders. In order to do that, one has to instantiate the contract by doing this:

    crowdsaleInstance = eth.contract([{inputs:[],name:'checkGoalReached',outputs:[{name:'response',type:'bytes32'}],type:'function',constant:false},{inputs:[],name:'deadline',outputs:[{name:'',type:'uint256'}],type:'function',constant:true},{inputs:[],name:'beneficiary',outputs:[{name:'',type:'address'}],type:'function',constant:true},{outputs:[{name:'response',type:'bytes32'}],type:'function',constant:false,inputs:[{name:'_beneficiary',type:'address'},{name:'_fundingGoal',type:'uint256'},{name:'_deadline',type:'uint256'},{name:'_price',type:'uint256'},{name:'_reward',type:'address'}],name:'setup'},{constant:true,inputs:[],name:'tokenReward',outputs:[{name:'',type:'address'}],type:'function'},{constant:true,inputs:[],name:'fundingGoal',outputs:[{type:'uint256',name:''}],type:'function'},{inputs:[],name:'price',outputs:[{type:'uint256',name:''}],type:'function',constant:true},{constant:true,inputs:[],name:'amountRaised',outputs:[{name:'',type:'uint256'}],type:'function'},{constant:true,inputs:[],name:'numFunders',outputs:[{name:'',type:'uint256'}],type:'function'},{inputs:[{name:'',type:'uint256'}],name:'funders',outputs:[{name:'addr',type:'address'},{name:'amount',type:'uint256'}],type:'function',constant:true},{inputs:[],name:'admin',outputs:[{name:'',type:'address'}],type:'function',constant:true},{inputs:[],type:'constructor'}]).at(registrar.addr('mycrowdsale'))

**Tip: check the section on how to register your token ABI on the last chapter on how to upload the abi on the network so all the user needs is the name or address of the contract.**

Once instantiated, anyone can check the progress of the contract by calling one of it's functions, like this:

    "The current funding at " +( 100 *  crowdsaleInstance.amountRaised.call() / crowdsaleInstance.fundingGoal.call()) + "% of its goals. Currently, " + crowdsaleInstance.numFunders.call() + " funders have contributed a total of " + web3.fromWei(crowdsaleInstance.amountRaised.call(), "ether") + " ether. The deadline is at " + Date(crowdsaleInstance.deadline.call())


Once the deadline is passed someone has to wake up the contract to have the funds sent to either the beneficiary or back to the funders (if it failed). This happens because there is no such thing as an active loop or timer on ethereum so any future transactions must be pinged by someone.

    crowdsaleInstance.checkGoalReached.sendTransaction({from:eth.accounts[1], gas: 1000000, gasPrice:web3.toWei(0.001, "finney")})




# Democracy DAO



So far you created a tradeable token and you successfully distributed it among all those who were willing to help fundraise a 100 ethers. That's all very interesting but what exactly are those tokens for?  Why would anyone want to own or trade it for anything else valuable? If you can convince your new token is the next big money maybe others will want it, but so far your token offers no value per se. We are going to change that, by creating your first decentralized autonomous organization, or DAO.

Think of the DAO as the constitution of your country, the executive branch of it's government or maybe like a completely robotic middle manager for an organization. The DAO receives the money that your organization raises via any means, keeps it safe and uses it to fund whatever it's citizens want. The robot is incorruptible, it will never defraud the bank, never create secret plans, never use the money for anything other than what it's constituents voted on. The DAO will never disappear, never run away and cannot be controlled by anyone other than it's own citizens.

The token we created using the crowdsale is the only citizen document needed. Anyone who holds any token is able to create and vote on proposals. Similar to being a shareholder in a company, the token can be traded on the open market and the vote is proportional to amounts of tokens the voter holds.  

Take a moment to dream about the revolutionary possibilities this would allow, and now you can do it yourself, in under a 100 lines of code:


### The Code

    contract token { mapping (address => uint) public balances;   function token() { }   function sendToken(address receiver, uint amount) returns(bool sufficient) {  } }

    contract Democracy {
      
      uint public minimumQuorum = 10;
      uint public debatingPeriod = 7 days;
      token voterShare;
      uint public numProposals = 0;
      address public founder;
      
      mapping (uint => Proposal) public proposals;
        
      struct Proposal {
        address recipient;
        uint amount;
        bytes32 data;
        bytes32 descriptionHash;
        uint creationDate;
        uint numVotes;
        uint quorum;
        bool active;
        mapping (uint => Vote) votes;
        mapping (address => bool) voted;
      }
      
      struct Vote {
        int position;
        address voter;
      }
      
      function Democracy() {
        founder = msg.sender; 
      }
      
      function setup(address _voterShareAddress){
        if (msg.sender == founder && numProposals == 0) {
          voterShare = token(_voterShareAddress);
        }   
      }
      
      function newProposal(address _recipient, uint _amount, bytes32 _data, bytes32 _descriptionHash) returns (uint proposalID) {
        if (voterShare.balances(msg.sender)>0) {
          proposalID = numProposals++;
          Proposal p = proposals[proposalID];
          p.recipient = _recipient;
          p.amount = _amount;
          p.data = _data;
          p.descriptionHash = _descriptionHash;
          p.creationDate = now;
          p.numVotes = 0; 
          p.active = true;
        } else {
          return 0;
        }
      }
      
      function vote(uint _proposalID, int _position) returns (uint voteID){
        if (voterShare.balances(msg.sender)>0 && (_position >= -1 || _position <= 1 )) {
          Proposal p = proposals[_proposalID];
          if (!p.voted[msg.sender]) {
            voteID = p.numVotes++;
            Vote v = p.votes[voteID];
            v.position = _position;
            v.voter = msg.sender; 
            p.voted[msg.sender] = true;
          }
        } else {
          return 0;
        }
      }
      
      function executeProposal(uint _proposalID) returns (uint result) {
        Proposal p = proposals[_proposalID];
        /* Check if debating period is over */
        if (now > p.creationDate + debatingPeriod && p.active){   
          uint yea = 0;
          uint nay = 0;
          /* tally the votes */
          for (uint i = 0; i <=  p.numVotes; i++) {
            Vote v = p.votes[i];
            uint voteWeight = voterShare.balances(v.voter); 
            p.quorum += voteWeight;

            if (v.position > 0) {
              yea += voteWeight;
            } if (v.position < 0) {
              nay += voteWeight;
            }
          }
          /* execute result */
          if (p.quorum > minimumQuorum && yea > nay ) {
            p.recipient.call.value(p.amount)(p.data);
            p.active = false;
          } else if (p.quorum > minimumQuorum && nay > yea) {
            p.active = false;
          }
          return yea - nay;
        }
      }
    }

There's a lot of going on but if you have ever read any kind of code this one should be easily understandable. The rules of your country are very simple: anyone with at least one token can create proposals to send funds from the country's account. After a week of debate and votes, if it has received votes totally at least 100 tokens and has more approvals than rejections, the funds will be sent. If the quorum hasn't been met or it ends on a tie, then voting is kept until it's resolved. Otherwise, the proposal is locked and kept for historical purposes.

So let's recap what this means: in the last two sections you created 10,000 tokens, sent 1,000 of those to another account you control, 2,000 to a friend named Alice and distributed 5,000 of them via a crowdsale.  This means that you no longer control over 50% of the votes in the DAO, and if Alice and the community bands together, they can outvote any spending decision on the 100 ethers raised so far. This is exactly how a democracy should work. If you don't want to be a part of your country anymore the only thing you can do is sell your own tokens on a decentralized exchange and opt out, but you cannot prevent the others from doing so.

### Set Up

So open your console and let's get ready to finally put your country online:

      var daoSource = 'contract token { mapping (address => uint) public balances; function token() { } function sendToken(address receiver, uint amount) returns(bool sufficient) { } } contract Democracy { uint public minimumQuorum = 10; uint public debatingPeriod = 7 days; token voterShare; uint public numProposals = 0; address public founder; mapping (uint => Proposal) public proposals; struct Proposal { address recipient; uint amount; bytes32 data; bytes32 descriptionHash; uint creationDate; uint numVotes; uint quorum; bool active; mapping (uint => Vote) votes; mapping (address => bool) voted; } struct Vote { int position; address voter; } function Democracy() { founder = msg.sender; } function setup(address _voterShareAddress){ if (msg.sender == founder && numProposals == 0) { voterShare = token(_voterShareAddress); } } function newProposal(address _recipient, uint _amount, bytes32 _data, bytes32 _descriptionHash) returns (uint proposalID) { if (voterShare.balances(msg.sender)>0) { proposalID = numProposals++; Proposal p = proposals[proposalID]; p.recipient = _recipient; p.amount = _amount; p.data = _data; p.descriptionHash = _descriptionHash; p.creationDate = now; p.numVotes = 0; p.active = true; } else { return 0; } } function vote(uint _proposalID, int _position) returns (uint voteID){ if (voterShare.balances(msg.sender)>0 && (_position >= -1 || _position <= 1 )) { Proposal p = proposals[_proposalID]; if (!p.voted[msg.sender]) { voteID = p.numVotes++; Vote v = p.votes[voteID]; v.position = _position; v.voter = msg.sender; p.voted[msg.sender] = true; } } else { return 0; } } function executeProposal(uint _proposalID) returns (uint result) { Proposal p = proposals[_proposalID]; /* Check if debating period is over */ if (now > p.creationDate + debatingPeriod && p.active){ uint yea = 0; uint nay = 0; /* tally the votes */ for (uint i = 0; i <= p.numVotes; i++) { Vote v = p.votes[i]; uint voteWeight = voterShare.balances(v.voter); p.quorum += voteWeight; if (v.position > 0) { yea += voteWeight; } if (v.position < 0) { nay += voteWeight; } } /* execute result */ if (p.quorum > minimumQuorum && yea > nay ) { p.recipient.call.value(p.amount)(p.data); p.active = false; } else if (p.quorum > minimumQuorum && nay > yea) { p.active = false; } return yea - nay; } } }'

      var daoCompiled = eth.compile.solidity(daoSource);
      var daoAddress = eth.sendTransaction({data: daoCompiled.Democracy.code, from: eth.accounts[0], gas:1000000, gasPrice: web3.toWei(0.001, "finney")});

Wait a minute until the miners pick it up. It will cost you about 0.6 ethers in current market price. Once it's picked up it's time to instantiate it and set it up, by pointing it to the correct address of the token contract you created previously. Let's also register a name for your contract so it's easily accessible (don't forget to check your name availability with registrar.addr("nameYouWant") before reserving!)

    var name = "MyPersonalCountry"
    registrar.reserve.sendTransaction(name, {from: eth.accounts[0]})
    var daoInstance = eth.contract(daoCompiled.Democracy.info.abiDefinition).at(daoAddress);
    daoInstance.setup.sendTransaction(registrar.addr("MyFirstCoin"),{from:eth.accounts[0]})

Wait for the previous transactions to be picked up and then:

    registrar.setAddress.sendTransaction(name, daoAddress, true,{from: eth.accounts[0]});

Test the parameters by doing these commands:

    daoInstance.numProposals.call();
    daoInstance.proposals.call();
    daoInstance.founder.call()

If everything is setup then your DAO should return a proposal count of 0 and an address marked as the "founder". While there are still no proposals, the founder of the DAO can change the address of the token to anything it wants. 

### Interacting with the DAO

After you are satisfied with what you want, it's time to get all that ether you got from the crowdfunding and into your new country:

    eth.sendTransaction({from: eth.accounts[1], to: daoAddress, value: web3.toWei(100, "ether")})

This should take only a minute and your country is ready for business! Now, as a first priority, your country needs a nice flag, but unless you are a flag expert, you have no idea how to do that. For the sake of argument let's say you find that your friend Bob is a great flag designer who's willing to do it for only 10 ethers, so you want to propose to hire him to design a flag. 

    recipient = registrar.addr("bob");
    amount =  web3.toWei(10, "ether");
    shortNote = "Flag Design";
    daoInstance.newProposal.sendTransaction( recipient, amount, shortNote, '', {from: eth.accounts[0], gas:1000000, gasPrice: web3.toWei(0.001, "finney")})

After a minute, anyone can check the proposal recipient and amount by executing these commands:

    daoInstance.numProposals.call()

Unlike most governments, your country's government is completely transparent and easily programmable. As a small demonstration here's a snippet of code that goes through all the current proposals and prints what they are and for whom:

    function checkAllProposals() {  
      for (i = 0; i< daoInstance.numProposals.call(); i++ ) { 
    var p = daoInstance.proposals.call(i)
    console.log("Proposal #" + i + "  Send " + web3.fromWei( p[1], "ether") + " ether to address " + p[0] + " for " + p[2]); 
    }
    }
    checkAllProposals();

A concerned citizen could easily write a bot that periodically pings the blockchain and then publicizes any new proposals that were put forth, guaranteeing total transparency.

Now of course you want other people to be able to vote on your proposals. You can check the crowdsale tutorial on the best way to register your contract app so that all the user needs is a name, but for now let's use the easier version. Anyone should be able to instantiate a local copy of your country in their computer by using this giant command: 


    daoInstance = eth.contract([{outputs:[{name:'recipient',type:'address'},{name:'amount',type:'uint256'},{name:'data',type:'bytes32'},{name:'descriptionHash',type:'bytes32'},{name:'creationDate',type:'uint256'},{name:'numVotes',type:'uint256'},{name:'quorum',type:'uint256'},{name:'active',type:'bool'}],type:'function',constant:true,inputs:[{name:'',type:'uint256'}],name:'proposals'},{constant:false,inputs:[{type:'uint256',name:'_proposalID'}],name:'executeProposal',outputs:[{name:'result',type:'uint256'}],type:'function'},{constant:true,inputs:[],name:'debatingPeriod',outputs:[{name:'',type:'uint256'}],type:'function'},{constant:true,inputs:[],name:'numProposals',outputs:[{name:'',type:'uint256'}],type:'function'},{name:'founder',outputs:[{name:'',type:'address'}],type:'function',constant:true,inputs:[]},{constant:false,inputs:[{type:'uint256',name:'_proposalID'},{name:'_position',type:'int256'}],name:'vote',outputs:[{name:'voteID',type:'uint256'}],type:'function'},{outputs:[],type:'function',constant:false,inputs:[{name:'_voterShareAddress',type:'address'}],name:'setup'},{type:'function',constant:false,inputs:[{type:'address',name:'_recipient'},{name:'_amount',type:'uint256'},{name:'_data',type:'bytes32'},{name:'_descriptionHash',type:'bytes32'}],name:'newProposal',outputs:[{name:'proposalID',type:'uint256'}]},{type:'function',constant:true,inputs:[],name:'minimumQuorum',outputs:[{name:'',type:'uint256'}]},{inputs:[],type:'constructor'}]},{type:'constructor',inputs:[]}]).at(registrar.addr('MyPersonalCountry'))

Then anyone who owns any of your tokens can vote on the proposals by doing this:

    var proposalID = 0;
    var position = 1; // +1 for voting yea, -1 for voting nay, 0 abstains but counts as quorum
    daoInstance.vote.sendTransaction(proposalID, position, {from: eth.accounts[0]});


Unless you changed the basic parameters in the code, any proposal will have to be debated for at least a week until it can be executed. After that anyone—even a non-citizen—can demand the votes to be counted and the proposal to be executed. The votes are tallied and weighted at that moment and if the proposal is accepted then the ether is sent immediately and the proposal. If the votes end in a tie or the minimum quorum hasn’t been reached, the voting is kept open until the stalemate is resolved. If it loses, then it's archived and cannot be voted again.

    var proposalID = 0;
    daoInstance.executeProposal.sendTransaction(proposalID, {from: eth.accounts[0]});


If the proposal passed then you should be able to see Bob's ethers arriving on his address:

    eth.getBalance(registrar.addr("bob"));


Try for yourself:  This is a very simple democracy contract, which could be vastly improved: currently, all proposals have the same debating time and are won by direct vote and simple majority.  Can you change that so it will have some situations, depending on the amount proposed, that the debate might be longer or that it would require a larger majority? Also think about some way where citizens didn't need to vote on every issue and could temporarily delegate their votes to a special representative. You might have also noticed that we added a tiny description for each proposal. This could be used as a title for the proposal or could be a hash of a larger document describing it in detail.

### Let's go exploring!

You have reached the end of this tutorial, but it's just the beginning of a great adventure. Look back and see how much you accomplished: you created a living, talking robot, your own cryptocurrency, raised funds through a trustless crowdfunding and used it to kickstart your own personal country. 

For the sake of simplicity, the simple democratic organization you created can only send ether around, the native currency of ethereum. While that might be good enough for some, this is only scratching the surface of what can be done. In the ethereum network contracts have all the same rights as any normal user, meaning that your organization could be programmed in such way that it could do any of the transactions that you executed coming from your own accounts. 

### What could happen next?

* The greeter contract you created at the beginning could be improved to charge ether for its services and could funnel those funds into the DAO.

* The tokens you still control could be sold on a decentralized exchange or traded for goods and services to fund further develop the first contract and grow the organization.

* Your DAO could own it's own name on the name registrar, and then change where it's redirecting in order to update itself if the token holders approved.

* The organization could hold not only ethers, but any kind of other coin created on ethereum, including assets whose value are tied to the bitcoin or dollar. 

* The DAO could be programmed to allow a proposal with multiple transactions, some scheduled to the future. 
It could also own shares of other DAO's, meaning it could vote on larger organization or be a part of a federation of DAO's

* The Token Contract could be reprogrammed to hold ether or to hold other tokens and distribute it to the token holders, linking therefore the value of the token to the value of other assets, so paying dividends could be accomplished by simply moving funds to the token address

This all means that this tiny society you created could grow, get funding from third parties, pay recurrent salaries, own any kind of cryptoassets and even use crowdsales to fund its activities. All with full transparency, complete accountability and complete immunity from any human interference. While the network lives the contracts will execute exactly the code they were created to execute, without any exception, forever.

So what will your contract be? Will it be a country, a company, a non-profit group? What will your code do? 

That's up to you.