

# What are NFTs?

NFTs(Non-fungible Token) have taken the world by storm, hitting $25 billion dollar sales in 2021. So what is an NFT and problems do they solve?

Before we explore these questions we need to look at the double spend problem. Prior to Bitcoin's blockchain it was challenging to figure out how to prevent someone from copying and pasting a digital dollar or any digital asset like an MP3 or a digital piece of art work. If you were to buy a digital piece of art work, there was no straight forward to identify that the art work is a one of one collection that you are the sole owner of us.

![Credit: BitPanda](https://cadena.incl.us/wp-content/uploads/2022/01/double-spending.png)

*Credit: BitPanda*

The blockchain solved this problem by verfiying every transaction and making transactions irrerversable and immutable but it still didn't solve the "fungible" aspect of digital ownership. Meaning while you can own Bitcoin, the qualities and properties of your Bitcoin are the same as every other person's Bitcoin. We'll come back to how this was solved on Ethereum but first we need to talk about ERC-20 tokens.

The Ethereum blockchain allowed for smart contracts (think of this as a contract written in code) which then gave rise for the ability for anyone to launch their own cryptocurrency on top of the Ethereum blockchain via the [ERC-20 Token](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/) standard. Introduced in 2015 the ERC-20 standard provided a set of standardize rules for how tokens should be created on top of Ethereum but these tokens were still fungible, meaning noting distuginish my Dogecoin from yours. 

Meet the ERC-721 standard which launched in 2017. This standard allowed for the creating of "none-fungible" tokens, meaning there can only be a one of a kind version of a token with a unique set of properties and attributes. Let's take for instance we're creating an NFT of a video game character. This characters metadata can include the url to where the JPEG file is stored of the character, the name of the character, any special attributes and properties like how many lives does this character have, can this character only use certain items in the game, etc. Cryptokittens was one of the early projects to adopt the standard for their virtual cats.

 Its a common misconception that NFTs can only be JPEGs, they can be any digital file you attach to them and even be paired with real world objects. In fact the NFT standard just holds the meta data which  properties, and attributes of a collection on the blockchain or "on chain". So what you commonly here is an "NFT" is really just an item's metadata stored on the blockchain following the ERC-721 standard that ensures the item is unique. This metadata can have different combinations that can be used to generate new characters on the fly to form an entire collection.

Many of these JPEGs are stored on third party decentralized file services like Arweave, Threefold, IPFS via gateways like Pinata, and even centralized services like Amazon Web Services (AWS) . We will get into the storage aspects of NFTs in the upcoming lessons.  The best way to understand ERC is diving into the code. Let's look at a [sample of an ERC-721 token smart](https://docs.openzeppelin.com/contracts/3.x/erc721) contract using the OpenZepplin library that is used to create unique items for video game players . 

**Note**: This lesson also presupposes you've completed the Ethereum 101 Course or know the basics of smart contracts, working with Ethers.js, and know another coding language like JavaScript. This example is a high level overview of the ERC-721 smart contract, in future lessons we will go more in depth.

```solidity
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC721/ERC721Full.sol";
import "@openzeppelin/contracts/drafts/Counters.sol";

contract GameItem is ERC721Full {

    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    constructor() ERC721Full("GameItem", "ITM") public {}

    function awardItem(address player, string memory tokenURI) public returns (uint256) {
        _tokenIds.increment();

        uint256 newItemId = _tokenIds.current();
        _mint(player, newItemId);
        _setTokenURI(newItemId, tokenURI);

        return newItemId;
    }
}
```



```solidity
pragma solidity ^0.8.0;
import "@openzeppelin/contracts/token/ERC721/ERC721Full.sol";
import "@openzeppelin/contracts/drafts/Counters.sol";
```

Here we're defining the version of solidity we're using. Then we're importing in OpenZeppelin's implemnetation of the ERC-721 standard which comes with some hand functions like checking who the owner of an NFT is and minting the NFTs.

```solidity
contract GameItem is ERC721Full {
    using Counters for Counters.Counter;
    Counters.Counter private _tokenIds;

    constructor() ERC721Full("GameItem", "ITM") public {}

    function awardItem(address player, string memory tokenURI) public returns (uint256) {
        _tokenIds.increment();

        uint256 newItemId = _tokenIds.current();
        _mint(player, newItemId);
        _setTokenURI(newItemId, tokenURI);

        return newItemId;
    }
}
```

On **line 1**, we name our contract GameItem and inhert the ERC721 library from OpenZepplin. On **line 2** we're inheritng all the methods from the [Counter library](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Counters.sol) and **line 3** we're inherting Counter which is of type of struct and has a default value of 0 that is of type uint. This can seem confusing but to clear things up take a look at this [line ](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/utils/Counters.sol#L15) from the Counters contract. We're then storing 0 in a private varible called "_tokenIds", as you can tell from the underscore at the start of this variable, it will be used as a function paramter.

On **line 5**, we're giving our token the name "GameItem" with the symbol "ITM".  On **line 7** we're using the **awardItem**() function that takes in the wallet address of the player, and a tokenURI. Think of a tokenURI as a unique identifier for our NFT meta data that we can reference to check and includes things like a link to our JPEG file, typically this is a JSON file. On **line 8** our tokenIds are incremented and **online 10** assigned to the variable **newItemID**.

 We then call the mint function **online 11** to actually create the NFT and assign it to the player. This costs gas and is the actual point where the NFT is created. We then assign the tokenURI to the item ID with [**setTokenURI**](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721#ERC721-_setTokenURI-uint256-string-) to ensure that our that item is one of a kind and finally we return the new item.

Lets look at what happens when we call this contract:

```bash
> gameItem.awardItem(playerAddress, "https://game.example/item-id-8u5h2m.json")
Transaction successful. Transaction hash: 0x...
Events emitted:
 - Transfer(0x0000000000000000000000000000000000000000, playerAddress, 7)
```

The player gets an NFT with an ID of 7 that includes the meta data from the JSON file (https://game.example/item-id-8u5h2m.json). We can prove this by running the ownerOf function that comes with our OpenZepplin contracts:

```bash
> gameItem.ownerOf(7)
playerAddress
> gameItem.tokenURI(7)
"https://game.example/item-id-8u5h2m.json"
```

Let's look at the meta data of what actually is in the json file:

```json
{
    "name": "Thor's hammer",
    "description": "Mjölnir, the legendary hammer of the Norse god of thunder.",
    "image": "https://game.example/item-id-8u5h2m.png",
    "strength": 20
}
```

So is an NFT just meta data in a JSON file with a unique ID that gets verified on chain that some how is valued in hundreds of thousands dollars 😲? Let us know what you think in the discord or you can tag us on twitter @cadenadev 🤔

