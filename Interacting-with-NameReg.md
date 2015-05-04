# Interacting with contracts: NameReg

All accounts are referenced in the network by their public address. But addresses are long, difficult to write down, hard to memorize and immutable. The last one is specially important if you want to be able to generate fresh accounts in your name, or upgrade the code of your contract. In order to solve this, there is a default name registrar contract which is used to associate the long addresses with short, human-friendly names.

So let’s pick a unique name for your country. Names have to use only alphanumeric characters and, cannot contain blank spaces. In future releases the name registrar will likely implement a bidding process to prevent name squatting but for now, it's a first come first served based. So as long as no one else registered the name, you can claim it.

`> registrar.reserve.sendTransaction("ethereumland", {from: primaryAccount});`

`> registrar.setAddress.sendTransaction("ethereumland", eth.accounts[0], true, 
{from: primaryAccount});`


Try for yourself: By typing just "registrar" you can see all the many functions available to the name registrar. Experiment with them. You can check a owner of any address by typing:

`> registrar.owner("ethereumland")`