### What's a Smart Contract?  

Think of a smart contract as code that gets executed on the Ethereum blockchain but there's more... One of the best definitions of a smart contract is from the Solidity documentation. 

> A contract in the sense of Solidity is a collection of code (its *functions*) and data (its *state*) that resides at a specific address on the Ethereum blockchain. [-Solidity Documention](https://docs.soliditylang.org/en/v0.8.9/introduction-to-smart-contracts.html) 

Keep this definition in mind as we progress, particulary state and functions. Smart Contracts can facilitate the creation of a new Token, an NFT, and has allowed for new industries to emerge like [DeFi](https://ethereum.org/en/defi/).

### Creating Your first Smart Contract

We will be using the Remix, IDE from Ethereum for this lesson. Go to [remix.ethereum.org](http://remix.ethereum.org).

Let’s create a file and call it HelloWorld.sol in the contracts folder. We’re going to give you the base template for a contract and then break it down line by line.

🚨 Tip: Closing your statements with a semicolon is required in Solidity.🚨

In line one we specify the license for our code. You can read more about the SPDX standard [here](https://spdx.dev/ids/). Then we definite the version of Solidity that we will be using in line two.

```solidity
// SPDX-License-Identifier: MIT*
pragma solidity ^0.8.3;
```

Next we define our contract using the **contract** keyword followed by the name of the contract and our code blocks. Note that a contract acts like a class in Solidity which allows for inheritance. Notice that we're using Pascal Case for the name of our contract.

```solidity
contract HelloWorld {
}
```

Next let's create a variable to store the classic greeting "Hello World!". You might have noticed the keyword **public** here. This allows for the creation of a getter function in the view part of our application and can be used with varibles and functions.

```solidity
contract HelloWorld {
	string public greeting = "Hello World!";
}
```

Now it's time to run our smart contract! Save your contract which will allow it to compile automatically. You should see a green check mark on the Solidity Compiler icon (second icon). Then click the Deploy and Run icon (the third icon down on the menu). 

For environment select "Injected Web3", then towards the button in the Contract section, select the contract you created, "HelloWorld.sol". Note sometimes there can be a lag if you don't see your contract. 

Before we go to the final steps, click on your MetaMask Wallet and towards the top center where it might say Ethereum Mainnet, click on that which will prompt a drop down menu. Then select Rinkeby Test Network, which will be our test next work moving forward. 

Next click **Deploy** in Remix which will prompt MetaMask to open up. Notice it will cost you some gas for this transaction. Every action in Ethereum is considered a transaction. Thankfully we’re using fake Ether so it won’t cost you anything. If you don't have fake Ether, be sure to get some fake from a faucet. 

Hit **Confirm** in MetaMask. In your console in the IDE you will see creation of your contract is pending. Open MetaMask and view your activity. You will see contract deployment. Click on that and it will open up a detailed activity log. Click on the first transaction in that log and it will take you to Etherscan. 

Notice the Rinkeby in front of the etherscan.io url. We're viewing transactions on this test network. There you have it, congrats on creating your first contract on the blockchain! 

Play around on Etherscan, look at the to and from address. Notice the contract address in the to field. This will come in handy in the future. 

Head back to Remix, go to Deploy & Run Transactions. You will see your contract in deployed contracts section towards the bottom. This is an instance of your contract. Notice our variable "greeting". Click on that and see what happens.

Notice in the console a call was triggered. This happens because we used **public** which as mentioned before creates a getter function.

🥳 Congrats again on creating your first ever smart contract! Feel free to take a screenshot and tweet about it. Be sure to tag [@cadenadev](https://twitter.com/cadenadev). 
