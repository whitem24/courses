# Anatomy of a Smart Contract
<script src="https://gist.github.com/saeedjabbar/8df7a329edbb92274bf1f08c8cf55ee9.js"></script>

### Mindset

One of the most important shifts in mindset when developing on the Ethereum blockchain is always thinking about gas and security as you code. Get into the mindset of what you write in your code will cost gas. Double check your code for overflows, underflows, and read, write, ownership permissions. These mistakes can be extremely costly. Loops are generally avoided and rarely used, maps are favored over arrays for faster lookup times that cost less gas. 

Note: This tutorial may seem lengthy but that's because of the code blocks.

### Bank Contract

We're going to go through the basics of smart contracts. To do this we will use an example of a Bank contract. Without even going to code, what are the essential functions of a bank? 

A bank needs to keep a total balance of their funds, allow customers to deposit money, and withdraw money, and also ensure that money is being sent and received by the right customers so that there is no theft of funds. Using this we can now expand upon it with code. 

I'm going to paste the final code here and then break it down line by line so you have a high level overview of what's happening. Then we will plug it into Remix to test. As a challenge for yourself, try to figure out what the contract does. Some of it is self-evident but if you're stuck on anything look at [Solidity by Example](https://solidity-by-example.org/).


```solidity
// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.0;

contract Bank {
    address public bankOwner;
    string public bankName;
    mapping(address => uint256) public customerBalance;

    constructor() {
        bankOwner = msg.sender;
    }

    function depositMoney() public payable {
        require(msg.value != 0, "You need to deposit some amount of money!");
        customerBalance[msg.sender] += msg.value;
    }

    function setBankName(string memory _name) external {
        require(
            msg.sender == bankOwner,
            "You must be the owner to set the name of the bank"
        );
        bankName = _name;
    }

    function withdrawMoney(address payable _to, uint256 _total) public {
        require(
            _total <= customerBalance[msg.sender],
            "You have insuffient funds to withdraw"
        );

        customerBalance[msg.sender] -= _total;
        _to.transfer(_total);
    }

    function getCustomerBalance() external view returns (uint256) {
        return customerBalance[msg.sender];
    }

    function getBankBalance() public view returns (uint256) {
        require(
            msg.sender == bankOwner,
            "You must be the owner of the bank to see all balances."
        );
        return address(this).balance;
    }
}

```



As usual we start our contract off with the license and solidity version. Then we create a contract called Bank{...}.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

contract Bank {
	.....
}
```



### Variables, Mappings and The Constructor Function

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

contract Bank {
	//declare state variables at contract level
    address public bankOwner;
    string public bankName;
    mapping(address => uint256) public customerBalance; //
    
    constructor() {
        bankOwner = msg.sender; //initialize state variable 
    }

}
```



### Variables

Solidity has local, state, and global variables. State variables are stored on the blockchain and will cost gas. Local variables are stored temporary in memory and not saved. They're typically used in functions. Global variables exist in the global namespace. One of the most important global variable is msg which includes msg.sender, msg.value, and msg.data which we will be using.  	

We're going to declare our variables `bankOwner` and `bankName` at the contract level which makes them state variables. We use the `address` keyword when we want to store an Ethereum address, in this case the address of our bank owner. 

Then we define another variable for our bank name with the type of `string`. Notice that these variables are both defined out side of any functions and both have a `public` keyword (visibility modifier). The `public` keyword will allow anyone to access these variables, automatically creating a getter for us which will come in handy for our front-end. Other visiblitiy modifiers include private, internal, and external.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

contract Bank {
	//declare state variables at contract level
    address public bankOwner;
    string public bankName;
}
```



### Mappings

Mappings are like Objects in JavaScript, Dictionaries in Python, and Hashes in Ruby. They're always stored in storage, even if they're inside functions, because they're state variables. They aren't iterable but offer faster look up times than arrays.

We create a mapping named CustomerBalance with the keys being the wallet address of our customers and value the amount of Ether they deposit in Wei. Wei is the smallest denomination of Ether (10^18), you can read more about it [here](https://www.investopedia.com/terms/w/wei.asp). Mappings are used often in smart contracts, [here's](https://solidity-by-example.org/mapping/) a reference to learn more. We could use the [suffix](https://docs.soliditylang.org/en/v0.8.10/units-and-global-variables.html) `ether` to simplify our numbers but we will stick to wei for learning purposes.

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

contract Bank {
	//declare state variables at contract level
    address public bankOwner;
    string public bankName;
    mapping(address => uint256) public customerBalance; //create mapping
}
```


### Constructor

We then define our constructor function to initialize our state variables, in this case just our bankOwner and not our bankName. Can you guess why not the bank name? The constructor is only executed once, when the contract deploys, and we want the address that executes the contract to be the only owner of that contract and saved permanently to the blockchain. Our bank name may change over time so we can always use a setter function to intialize this variable after the contract is deployed. 

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity ^0.8.0;

contract Bank {
	//declare state variables at contract level
    address public bankOwner;
    string public bankName;
    mapping(address => uint256) public customerBalance; //create mapping
    
    //constructor will only run once, when the contract is deployed
    constructor() {
        //we're setting the bank owner to the Ethereum address that deploys the contract
        //msg.sender is a global variable that stores the address of the account that initiates a transaction
        bankOwner = msg.sender;
    }
}
```



### Functions

We're going to look at a high level overview of the skeleton of the functions we defined, then go into detail with each one. Many of these functions are setter and getter functions. Setter functions change the value of our state variables and cost gas. Getter functions allow us to get the return value of our state variable. Both will be used for our Dapp interface. 

```solidity
// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.8.0;

contract Bank {

    address public bankOwner;
    string public bankName;
    mapping(address => uint256) public customerBalance;

    constructor() {
        bankOwner = msg.sender;
    }

    function depositMoney() public payable {
			...
    }

    function setBankName(string memory _name) external {
    	...
    }

    function withdrawMoney(address payable _to, uint256 _total) public payable {
    	...
    }
    
    function getCustomerBalance() external view returns (uint256) {
       ...
    }

    function getBankBalance() public view returns (uint256) {
			...
    }
}

```



### depositMoney()

We need a away for our customers to deposit money to our bank so we created a function called depositMoney. Let's break down the `depositMoney` function.  We have the name of our function followed by the parameter it's taking which has a type of `uint256` (256-bit unsigned integer, uint is the alias for uint256 ). Note that the parameter starts with an `_` (underscore). This isn't required but has become a convention, to distinguish parameters from other variables. Parameters are also temporarily stored in memory.

After our paramters, we have a visibility specifier of public which allows the function to be called internally or externally. Then we have our modifier `payable`. As the name implies we need this modifier to recieve money in our contract. You can also use the receive function introduced in Solidity 0.6, you can read more about that [here](https://blog.soliditylang.org/2020/03/26/fallback-receive-split/).

Here's a quick cheat sheet for the specifiers in Solidity, you can read more about them [here](https://ethereum.stackexchange.com/questions/32353/what-is-the-difference-between-an-internal-external-and-public-private-function/112106).


| Visibility Specifiers | What they do?                                                 |
| --------------------- | ------------------------------------------------------------- |
| **public**            | Everyone can access                                           |
| **private**           | Can be accessed only from within this contract                |
| **internal**          | Only this contract and contracts deriving from it can access  |
| **external**          | Cannot be accessed internally, only externally (saves gas)    |

*Credit: Dragos Barosan*

Let's move on to the `require()` line. We could use `if/else` for our checks but require is preferred. Require is like a `try, catch` in javascript. First we check for the validity of a certain condition, and then we set the error if that condition isn't met. 

Now notice we're using `msg.value` in our `require()`. `msg` is a global variable that allows us to access properties like the `sender`, the address that initiated the transaction, and `value`, the amount of Ether in wei being sent. Here we're doing a check to ensure the user doesn't send us 0 wei.

Lastly, we look through our customerBalance mappings to see if the address of the `msg.sender` exists and then add the total the deposited to their balance. Notice how mappings are more efficient than an array here, we would have had to loop through an array to find the `msg.sender`, which is costly on Ethereum. 

```solidity
     function depositMoney() public payable {
        require(msg.value != 0, "You need to deposit some amount of money!");
        customerBalance[msg.sender] += msg.value;
    }
```



### setBankName()

We declared the state variable `bankName` earlier in our code, now we want to set the value of that state variable. We will do so with a setter function which can be called externally, hence why I am using the `external` keyword here. We could use `public` but since we aren't calling this function any where in our contract, we can save gas using external. Setter functions will cost us gas.

I am also being explicit for demonstration purposes by ensuring `_name` is stored in memory. Lastly we do a check so that only the address of the bank owner can change the bank name (this is why we stored it in a permanent state variable earlier). Then lastly we assign the new bank name to the `bankName` variable. You may be wondering, well how do we get the `bankName`? A getter for the variable is already created for us when we initilaize the variable and made it public.

```solidity
function setBankName(string memory _name) external {
	require(msg.sender == bankOwner, "You must be the owner to set the name of the bank");
	bankName = _name;
}
```



### **withdrawMoney**()

This is self-explanatory but in short, we check if the balance the customer wants to withdraw is equal or less than their total balance, so they can't over-draw funds. Then we use our mapping to find that customer and deduct the amount they want to withdraw from their balance. Lastly we transfer the money to the customer's address. The transfer function is built into Solidity and transfers money to an address. There are other ways to transfer money but we wanted to be brief for learning purposes. If you're interested in a more secure way, read this [article](https://consensys.net/diligence/blog/2019/09/stop-using-soliditys-transfer-now/).

```solidity
    function withdrawMoney(address payable _to, uint256 _total) public {
        require(_total <= customerBalance[msg.sender], "You have insuffient funds to withdraw");
        customerBalance[msg.sender] -= _total;
        _to.transfer(_total);
    }
```

### **getCustomerBalance()**

Next we create another getter to get the balance of the wallet calling our contract. 

```solidity
     function getCustomerBalance() external view returns (uint256) {
        return customerBalance[msg.sender];
    }
```



### **getBankBalance()**

Lastly we create another getter function to get the balance of the entire bank. 

```solidity
    function getBankBalance() public view returns (uint256) {
        //we want ony the bank owner to see all balances
        require(msg.sender == bankOwner,"You must be the owner of the bank to see all balances.");
        return address(this).balance;
    }
```

Deploy your contract to remix and experiment with it. Play around with the getters (blue buttons), setters (orange buttons), and transactions (red buttons). You can use this [calculator](https://eth-converter.com/) to convert Eth to Wei. Here's a video of the deployment of the contract to Remix.  You can test quickly with the JavaScript VM enviromment and then do another test with the Injected Web3 environment. Here's the completed code for this [lesson](https://gist.github.com/saeedjabbar/8df7a329edbb92274bf1f08c8cf55ee9).

<iframe className="mx-auto" width="100%" height="515"  src="https://www.youtube.com/embed/CvY331RwCDk?rel=0" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

### Wrapping Up

Be sure you understand the concepts presented here before moving on to the next lessons. Challenge yourself by re-writing the contract again and then explain it to another developer line by line what's occurring. One of the fastest ways to learn is teaching. 

There's one huge vulnerbility in our code. Have you spot what it is? What if the bank owner is secretly a theif and can just disappear with all our funds? This happens more often than we think in web3 and is often referred to "pulling the rug". In fact $7.7 billion dollars were lost to these type of scams in [2021](https://indianexpress.com/article/technology/crypto/rug-pull-cryptocurrency-scam-costed-investors-over-7-7-billion-in-2021-chainalysis-7677725/#:~:text=A%20rug%20pull%20is%20a,run%20away%20with%20investors'%20funds.). 

This has given rise to third party contract auditors like OpenZeppelin, Quantstamp, and ConsenSys Diligence. You may start to see why we need a trustless system like the blockchain now, where we don't need to rely on any one party to manage our funds.
