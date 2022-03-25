# Getting Started

We want you to avoid tutorial hell and jump into building projects from day one. If you don’t know something as you move along, do a quick google search or ask in our discord channel.

Avoid getting stuck and going down rabbit holes. One of your goals should be to get a high level understanding of how all the moving pieces of developing on the blockchain come together. On your second go, you can go more in depth or come up with your own projects.

Our main goal is to get to done. If you’re stuck, you have our permission to copy and paste the sample code just so that you can move on to the next lesson and get to done. 

## A Note on Security 🚨
***

There are numerous sophisticated attempts to hack, phish and steal crypto currencies since there is no central authority to over see the blockchain and you cannot undo any transaction. 

**Double check** all urls, even the urls you click through searching on Google to make sure they are accurate. Never share your seed and private keys with anyone and make sure to back them up in multiple secure places. 

A good rule of thumb is not to trust anyone in the MetaVerse, impersonation is a common mechanism for theft. Always triple check links and wallets you send digital assets to and do not store any credentials in your main code base that is pushed to GitHub. We will be using [dotenv](https://www.npmjs.com/package/dotenv) to store our credentials locally only. 

## Prerequisites
***

**Wallets**

To get started you will need a wallet. Your wallet will act as your login to access the Ethereum network. Your public key is like your username and your private key is like your password. 

We recommend using [MetaMask](https://metamask.io/). Please store your seed and private keys in a secure place. Create a test account just for this project. We will be using the Rinkeby Test Network, which is a test-network that mimics the the functionality of the real network. This allows us to use Fake Ether as well.

Click on the fox icon in your browser, then click Ethereum Mainnet located at the top center, then in the drop down menu click Rinkeby Test Network. 
(If you dont find Rinkeby Test Network in the dropdown menu, go to settings => advanced => then switch on the 'show test networks' button. Hopefully the Rinkeby Test Network will showup along with other test networks).

Next you will need send your self some fake Ether via what’s known as a faucet. Here are a list of faucets, again double check these as sometimes the faucets are out of fake ether.

- Alchemy(fastest): https://www.rinkebyfaucet.com/
- MyCrypto: https://app.mycrypto.com/faucet
- Rinkeby: https://faucet.rinkeby.io
- MetaMask: https://faucet.metamask.io
- Chainlink: https://faucets.chain.link/rinkeby
- You can ask us in the discord if you run out of fake Ether.

**Solidity** 

Smart contracts are programmed in Solidity. Solidity is fairly easy to pick up, if you don’t know it, you will pick it up as we move along. It's concepts from C++, Python and JavaScript so it should be familiar enough. 

Here’s a cheat sheet to speed up your learning:

- A high level overview of the Solidity language: [https://solidity-by-example.org/](https://solidity-by-example.org/)
- CryptoZombies is another amazing resource to pick up Solidity as well: https://cryptozombies.io

**JavaScript** 

You’ll need to have an intermediate understanding of JavaScript or another programming language to get through this tutorial. If you are new to coding, we recommend learning a coding language like JavaScript or Python first. We will be using some Javascript libraries like Ethers.js to interact with our smart contract. 

**Code Editor**

You can use your favorite code editor but download the package that allows for Solidity syntax highlighting. For some lessons we will use Remix, an online development environment for smart contracts ([remix.ethereum.org/](http://remix.ethereum.org/)).

➡️ **Action Item** Share with us your public address in the [discord](https://discord.gg/UQayXxzazc) and let us know what you learned by setting up your wallet. 
