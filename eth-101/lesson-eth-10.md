# Create Your Own Meme Coin 

Now that you're familiar with the basics of smart contracts and working with Ethers.js we can explore some fun topics like creating your own meme coin!  We will be using the **ERC-20** token standard which is a go to standard to release tokens on the Ethereum blockchain. You can read more about this standard [here](https://eips.ethereum.org/EIPS/eip-20) in the official documentation. 

ERC-20 standards allow us to create fungible tokens, meaning there can be many people who all hold our tokens at the same market value, let's say $5 and there is nothing unqiue to each token. [ERC-721](https://eips.ethereum.org/EIPS/eip-721) on the other hand allows us to create Non-fungible tokens commonly reffered as NFT's where we can program our tokens to have a limited amount and be unique to each individual owner. 

Instead of writing our own ERC-20 from scratch which can expose us to security vulnerabilities we will use OpenZepplin's ERC-20 library which offers an audited [boilerplate](https://docs.openzeppelin.com/contracts/4.x/erc20) and a ton of helpful built in [functions](https://docs.openzeppelin.com/contracts/2.x/api/token/erc20). You can even use this [wizard](https://docs.openzeppelin.com/contracts/4.x/wizard) from OpenZepplin to map out your ERC-20 contracts. 

For my meme coin I will be creating Fox Coin 🦊 with a total supply of 1000 tokens and the the symbol FXC. You come up with a unique name for your coin, symbol, and total supply. Our contract will be simple, we will mint some tokens then be able to send them to our MetaMask users on the Rinkeby testnet. You can even airdrop some to your classmates.

Our completed contract is found below

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract MemeCoin is ERC20, Ownable, ERC20Burnable {
    event tokensBurned(address indexed owner, uint256 amount, string message);
    event tokensMinted(address indexed owner, uint256 amount, string message);
    event additionalTokensMinted(address indexed owner,uint256 amount,string message);

    constructor() ERC20("FoxCoin", "FXC") {
        _mint(msg.sender, 1000 * 10**decimals());
        emit tokensMinted(msg.sender, 1000 * 10**decimals(), "Initial supply of tokens minted.");
    }

    function mint(address to, uint256 amount) public onlyOwner {
        _mint(to, amount);
        emit additionalTokensMinted(msg.sender, amount, "Additional tokens minted.");
    }

    function burn(uint256 amount) public override onlyOwner {
        _burn(msg.sender, amount);
        emit tokensBurned(msg.sender, amount, "Tokens burned.");
    }
}
```

You might be thinking wow that's it, I can make the next big meme coin with just less than 30 lines of code (actually less than 10 lines but I added some extra examples for learning purposes)? This is possible because we're inherting a bunch of useful functions and variables from OpenZepplin. Take a look at the full [ERC20 contract](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol) we are inheriting to see all the functions and variables we're getting out of the box. We will use mint(), symbol(), name(), balance(), decimal() out of the box.

Let's create a new Hardhat project called coin-smartcontract. You can follow the steps for setting up a Hardhat project [here](https://app.cadena.dev/lesson/ethereum-101/lesson-eth-6/6). Once you setup Hardhat run the following to install OpenZepplin. Then create a contract called MemeCoin.sol in your contracts folder.

```bash
npm install @openzeppelin/contracts
```

Now let's look at our contract line by line:

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";
```

As usual we declare our liscenes and solidity version in lines 1 and 2 but line 3 is where the real magic happenes. We're importing the ERC20 library from OpenZepplin and as mentioned before we will be inheriting some useful [functions](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol) out of the box.

```solidity
contract MemeCoin is ERC20, Ownable, ERC20Burnable {
    event tokensBurned(address indexed owner, uint256 amount, string message);
    event tokensMinted(address indexed owner, uint256 amount, string message);
    event additionalTokensMinted(address indexed owner,uint256 amount,string message);
    .....
}
```

In `line 1` we are creating our contract called MemeCoin and using the `is` keyword to inherit the functionalities of the [ERC20](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol#L87), [Ownable](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol), and [ERC20Burnable](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/ERC20Burnable.sol) libaries into our contract. Take a look at the contracts by clicking the links in the previous sentence to get a sense of the functions and variables we will be using in our code. For instance we're inheriting the [burn()](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/extensions/ERC20Burnable.sol#L20)  function from **ERC20Burnable** to burn an excessive supply of our tokens if we needed to.

Then on lines 2 through 4, we set up a Solidity [event](https://solidity-by-example.org/events/) which lets us set up logging directly to the Ethereum blockchain. We start with the keyword event followed by the name of the events and the paramters we want to log to the blockchain. The `indexed` keyword is used for keywords that we want to use as a filter on the blockcahin. Events are followed by an emitter which triggers the log to be saved. The format for an emitter is as follows: 

```solidity
emit tokensBurned(msg.sender, amount, "Tokens burned.");
```

We can use events to lessen our gas costs, see more detail on this [here](https://media.consensys.net/technical-introduction-to-events-and-logs-in-ethereum-a074d65dd61e) and listen in real time on the front-end of our DAPP when a certain action happens, for example burning a token.

```solidity
constructor() ERC20("FoxCoin", "FXC") {
    _mint(msg.sender, 1000 * 10**decimals());
    emit tokensMinted(msg.sender, 1000 * 10**decimals(), "Initial supply of tokens minted.");
}
```

In `line 1` we're calling our ERC20 constructor which requires the name of our token and its ticker symbol as its parameters. Then in `line 2` we're pulling the [_mint](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol#L252) function from the ERC20 library which lets us mint our token and transfer it to ourselves via `msg.sender`.

 In this case we're minting 1000 tokens but notice that we are using `decimal()` . This function allows us to create fractional amounts of our currency, in this case 18 decimal places just like wei. `decimal()` actually just returns the number 18, you can see that [here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol#L87). Alternatively, we could have just written ` 1000 * 10**18` but why not get a little fancy 🙂 Then finally we set up an emitter to log our action to the blockchain.

```solidity
function mint(address to, uint256 amount) public onlyOwner {
    _mint(to, amount);
    emit additionalTokensMinted(msg.sender, amount, "Additional tokens minted.");
}
```

We create another mint function here in the event as there is a huge demand for our token and we want to add more supply. We're using the `onlyOwner` modifier here from the `Ownable.sol` library to restrict this action to the owner of the contract only. You can see this [here](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol#L8). We could of course do a `require` check here but we get a ton out of the box with the Ownable library like the ability to transfer the [ownership](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol#L23) of our contract. Then we end with an emiter to log that our tokens have been minted.

```solidity
function burn(uint256 amount) public override onlyOwner {
    _burn(msg.sender, amount);
    emit tokensBurned(msg.sender, amount, "Tokens burned.");
}
```

Notice we're using the `override` specifier here in line 1. This is used because there's already a burn() function specified in `ER20.sol` and we want to overwrite that with the burn function in `ERC20Burnable.sol`.  As usual, we log the amount of tokens that have been burned.  

## Deploy and Env Vairables

Be sure to also configure your deploy.js file in Hardhat and your .env variables if you haven't done that already. 

```javascript
const hre = require("hardhat");

async function main() {

  const [owner] = await hre.ethers.getSigners();
  const contractFactory = await hre.ethers.getContractFactory("MemeCoin");
  const contract = await contractFactory.deploy();
  await contract.deployed();

  console.log("MemeCoin Contract deployed to:", contract.address);
  console.log("MemeCoin Contract owner address:", owner.address);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });

```

```BASH
ALCHEMY_RINKEBY_URL=YOUR_ALCHEMY_RINKEBY_URL
RINKEBY_PRIVATE_KEY=YOUR_PRIVATE_KEY
```



## Wrapping Up 🎁

And that's a wrap! Go ahead and deploy your token in Hardhat or Remix. In MetaMask [click on import tokens](https://metamask.zendesk.com/hc/en-us/articles/360015489031-How-to-add-unlisted-tokens-custom-tokens-in-MetaMask), drop in your token address from the Rinkeby network and like magic it will pull in your token symbol and amounts. Feel free to spread the wealth with other classmates and show off on twitter your very own meme coin. Tag us at @cadenadev for a retweet.

Remeber you can also debug in Hardhat using the [console.log()](https://hardhat.org/tutorial/debugging-with-hardhat-network.html).

If you want to test your contract out, [Remix](https://remix.ethereum.org/) is a great way to do so as well since you get to test all the getters and setters without needing a front-end. If you do use remix, you may need the direct url of the contract library for it to work, i.e.: 

```solidity
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";
```

Congrats, perhaps your meme token might be the next to the moon 🚀🌙 Notice a lot of the functions we had to manualy create in our first lesson are done for us by OpenZepplin but we had to know the basics before we could get to the more advance stuff.
