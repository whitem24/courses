# Environment Setup 🛠

As a smart contract developer we need to compile, test, debug, and deploy our contracts. Then we need a front-end libary to help us interact with our smart contract. There are many development frameworks and front-end libaries that help us with this, the most popular being truffle and web3.js. 

While truffle and web3.js are used in many projects today, more and more developers are switching over to hardhat and ethers.js for smart contract development so we will be using this stack moving forward. We still recommend playing around with truffle and web3.js. There are even SDKs out there that abstract away the complexity of developing contracts so you can write everything in just JavaScript, but it's important to learn the basics first.

We will also be using [Alchemy](https://www.alchemy.com/) to broadcast our transactions to the testnet instead of Infura but we also recommend playing around with Infura. If you have a Python background, check out [Brownie](https://eth-brownie.readthedocs.io/en/stable/).

**Note:** This lesson will be lengthy so set aside 20mins-30mins. Feel free to take breaks in between.

### Setting Up Hardhat 👷🏾‍♀️

Hardhat is a suite of tools that allow us to build, debug, and deploy our smart contracts on the Ethereum blockchain. They have a huge [plugin](https://hardhat.org/plugins/) library and you can even `console.log()` right in your smart contract for debugging. Hardhat comes with its own local blockchain that you can test on, saving you a ton of time. 

If you would like to get a head start you can use the official Hardhat [documentation](https://hardhat.org/getting-started/) to install Hardhat. Let's get started.

1. First make sure you have node version 12 or above and npm installed. Then install hardhat.

```bash
node --version
npm --version
```

If you don't have node you can find the installaiton guides for Windows, macOS, and Linux [here](https://nodejs.org/en/download/).

2. Make a directory for our project called bank-smartcontract and cd into it. Then initalize npm for our project and install hardhat. We're installing dotenv to store our project's environment variables.

```bash
mkdir bank-smartcontract
cd bank-smartcontract
npm init -y
npm install dotenv
npm install --save-dev hardhat
```

3. Next run the following command to initialize a hardhat project and then choose the first option `Create a sample project`

```bash
npx hardhat
```

Now hit enter for all the options and pay special attention to all the depencies being installed.

![](https://cadena.incl.us/wp-content/uploads/2021/12/hardhat-options.png)

```bash
npm install --save-dev @nomiclabs/hardhat-waffle@^2.0.0 ethereum-waffle@^3.0.0 chai@^4.2.0 @nomiclabs/hardhat-ethers@^2.0.0 ethers@^5.0.0
```

We will do a quick breakdown of these dependencies and what they do.

| Dependencies | What they do?                                                     |
| ------------ | ----------------------------------------------------------------- |
| waffle       | Used for testing smart contracts                                  |
| chai         | Used for testing in Javascript                                    |
| ethers       | A javascript libary used to interact with the Ethereum blockchain.|

4. Run `npx hardhat` again and you will see the global options and pay special attention to the tasks, we will using some of these often. you can use `npx hardhat accounts` to see a list of test accounts we can work with.

![](https://cadena.incl.us/wp-content/uploads/2021/12/hardhat-tasks.png)

5. Now open up your project folder in your favorite editor, we will be using VS Code for this class. Install this [Solidity plugin](https://marketplace.visualstudio.com/items?itemName=JuanBlanco.solidity) for VS Code which allows us to format Solidity.

Let's check out the folder structure from Hardhat. `contracts` will be where our contracts go, `scripts` is where our scripts will live that allow us deploy our contract, and `test` is for any testing we would like to do.

6. Delete `Greet.sol` from the contract folder, delete `sample-script.js` from scripts, and `sample-test.js` from the test folder. 

We're almost at the finish line. We're now going to setup our hardhat config and our environment variables.

7. Go into hardhat.config.js delete everything and paste the following in and save:

```javascript
require('@nomiclabs/hardhat-waffle');
require('dotenv').config();

module.exports = {
  solidity: "0.8.0",
  networks: {
    rinkeby: {
      url: `${process.env.ALCHEMY_RINKEBY_URL}`,
      accounts: [`${process.env.RINKEBY_PRIVATE_KEY}`],
    } 
  }
};
```

This is straight forward, we're using solidity 8.0, the rinkeby test network, the url property is an Ethereum node which we will be using Alchemy for (more on this later) and in accounts will be the private key of an Ethereum account (more on this later).

We can access these enviroment globally using the following line of code:

```javascript
require('dotenv').config();
```

**🚨 SECURITY ALERT 🚨**

Before we proceed, create a metamask account that will only be used for this course as your private key will be needed to sign transactions. Anyone who has your private key can steal all funds from your wallet, hence why we're using dotenv to store our private keys.

8. Now we're going to setup our enviromment variables. Create a file called `.env` in the root of our project folder and paste the following in:

```javascript
ALCHEMY_RINKEBY_URL=YOUR_ALCHEMY_RINKEBY_URL
RINKEBY_PRIVATE_KEY=YOUR_PRIVATE_KEY
```

Switch your Metamask to the Rinkeby Testnet and paste in your private key. [Here's](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key) how you can get your private key. Again make this a throw-away account that will never be used for real transactions. We will need the private key to sign our transactions, deploy and make function calls to our contract.

### Setting up Alchemy 🧙🏽‍♀️ 

Next we need an Ethereum node to broadcast our contract transaction to the blockchain so miners can pick it up, mine it, and store it on the blockchain. For this we will be using [Alchemy](https://www.alchemy.com/). Head over here and sign up for an account and verify your email. 

On the `Select Ecosystem` page choose "Ethereum including L2s". On the `Create your first app` page Enter 'Bank DAPP' or whatever you would like to call your app and team name. The most importantly in the `network` drop down **choose Rinkeby**.

For the plan click free forever, click skip for now when they ask for credit card info, then hit continue for scaling policy. Click let's go to be taken to your dashboard. Click the name of your app "Bank DAPP", click view details and in the top right click view key. Copy and paste your HTTP key into the `ALCHEMY_RINKEBY_URL` section. Save your .env file.

To load up your account with fake eth, [here](https://app.cadena.dev/lesson/ethereum-101/lesson-eth-2/2) are some faucets from our previous lessons.

Great job getting this far! That was a lot, so feel free to take a breather 🌱
