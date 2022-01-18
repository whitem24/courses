# Deploying Our Contract

Now that we're all setup let's deploy our bank contract to the Rinkeby network. Be sure to have fake eth in your account from a faucet before we proceed.

1. First, copy the code from our bank contract lesson, create a file called "BankContract.sol" in the contracts folder in your hardhat project and paste your code in, then save the file. If you need a copy of the contract you can find it [here](https://gist.github.com/saeedjabbar/8df7a329edbb92274bf1f08c8cf55ee9).
2. Then we compile our contract with by running this command, remember this flow compile -> deploy.

```bash
npx hardhat compile
```

You should see `Compilation finished successfully`. If you don't for some reason run this command and then compile again. [Here's](https://hardhat.org/guides/compile-contracts.html) the official Hardhat documentation on compiling.

```
npx hardhat clean
```

 Notice after we compiled our contract, we have a new folder called `artifacts` that contains two other folders `build-info` and `contracts`. Open those folders up and take a look around. 

In contracts notice a file named Bank.json or what ever the name was of your smart contract. This will be super important because we will need it for our front-end to interact with our smart contract. This is essentially a JSON version of our smart contract. This is referred to as an ABI file, you can learn more about that [here](https://www.quicknode.com/guides/solidity/what-is-an-abi) and read more about artifacts [here](https://hardhat.org/guides/compile-contracts.html#artifacts).

3. Now it's time to deploy our contract. In your scripts folder, create a new file called deploy.js and paste the following code. I've included the comments that come out of the box with hardhat but we will also go through this line by line.

```javascript
// We require the Hardhat Runtime Environment explicitly here. This is optional
// but useful for running the script in a standalone fashion through `node <script>`.
//
// When running the script with `npx hardhat run <script>` you'll find the Hardhat
// Runtime Environment's members available in the global scope.
const hre = require("hardhat");

async function main() {
  // Hardhat always runs the compile task when running scripts with its command
  // line interface.
  //
  // If this script is run directly using `node` you may want to call compile
  // manually to make sure everything is compiled
  // await hre.run('compile');

  // We get the contract to deploy
  const [owner] = await hre.ethers.getSigners();
  const BankContractFactory = await hre.ethers.getContractFactory("Bank");
  const BankContract = await BankContractFactory.deploy();
  await BankContract.deployed();

  console.log("BankContract deployed to:", BankContract.address);
  console.log("BankContract owner address:", owner.address);
}

// We recommend this pattern to be able to use async/await everywhere
// and properly handle errors.
main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });

```

I'll be only focusing on these 4 lines since the rest is self-explanatory and the hardhat comments do a great job explaining it.

```javascript
  const [owner] = await hre.ethers.getSigners();
```

We're getting the contract owner from the ethers object. You can read more on Signers [here](https://hardhat.org/guides/waffle-testing.html#testing-from-a-different-account).

```javascript
  const BankContractFactory = await hre.ethers.getContractFactory("Bank");
```

This is where the magic happens, we get the contract using `getContractFactory` and our contract name. Then it is compiled and the output is the resulting files in the artifacts folder.

The next two lines may look similar but they're different.

```javascript
  const BankContract = await BankContractFactory.deploy();
```

`deploy()` deploys our contract on the local Hardhat network which we might want to use for debugging purposes.

```javascript
  await BankContract.deployed();
```

Finally we use `deployed()` to deploy to the actual blockchain of our choice. In this case the Rinkeby testnet.

------

After you copy and paste the code into deploy.js, run the following to deploy to the Rinkeby test network. If you're interested in deploying on a local hardhat node you can read more about that [here](https://hardhat.org/guides/deploying.html#deploying-your-contracts).

```
npx hardhat run scripts/deploy.js --network rinkeby
```

You should see something like the following. The contract address and the wallet address of the owner.

```bash
BankContract deployed to: 0x8DA9151adD3070fAc7c887B1f6FadA58CC2a12c5
BankContract owner address: 0x2C9e104F0966CA8AD51a228C236f32Da9783EAff
```

Well done 🥳 Your smart contract is now live on the Ethereum testnet. You can view it on the Rinkeby Etherscan [here](https://rinkeby.etherscan.io/) by dropping in the address of your contract. Notice it costs us a transaction fee (Txn fee). If you want to see your contract code on the blockchain, click contract (located next to transactions on the bottom left), then click decompile byte code. Currently you may have to wait north of an hour to see the actual code of your contract.

In the upcoming lessons we will be building our front-end to interact with our smart contract.

Note: If you have errors with compiling or deploying your contract try running the following command or delete the cache and artifacts folder manually. You can also drop us a note in the discord.

```bash
npx hardhat clean
```

