#### Addresses

The Ethereum bllobkchain is made up of "accounts" which you can think of like 
bank accounts. An account has a balance of Ether, and you can send and receive
Ether payments to other accounts, just like your bank account can wire
transfer money to other bank accounts.

Each account has an `address` which you can think of like a bank account number.
It is a unique identifier that points to that account, and it looks like :
```sol
0x0cE446255506E92DF41614C46F1d6df9Cc969183
// Address of CryptoZombies team (you can send ETH)
```
Overall, an address is owned by an owner (or a smart contract).

#### Mappings 

Like structs, mappings are another way to organize data inside Solidity.
Defininng a `mapping` looks like this :

```sol
// For a financial app, soring a uint that holds the user account balance
// This one must be public hence we use the "public" keyword
mapping (address => uint) public accountBalance;
// Or could be used to store / lookup usernames based on UserID
mapping (uint => string) userIDtoName;
```

It is essential a key-value store. Where all keys are of the same data type 
and all values also share the same datatype.

#### Msg.sender

In solidity, there are certain global variables that are available to all functions.
ONe of these is `msg.sender`, which refers to the `address` of the person (or 
smart contract) who called the current function.

Here's an example :
```sol
mapping (address => uint) favoriteNumber;

function setMyNumber(uint _myNumber) public {
    // Update our `favoriteNumber` mapping to store `_myNumber` under msg.sender
    favoriteNumber[msg.sender] = _myNumber;
    // Syntax is similar to arrays with indexes
}

function whatIsMyNumber() public view return (uint) {
    // Retrieve the vale stored in the sender's address 
    // Will be `0` if the sender hasn't called `setMyNumber` yet
    return favoriteNumber[msg.sender];
}
```

Using `msg.sender` gives you the security of the Ethereum blockchain -- the only way
someone can modify someone else's data would be to steal the private key associated
with their Ethereum address.

#### Require 

The `require` keyword makes it so that the function will throw an error and stop 
executing if some condition is not true. It is a guard clause equivalent. 
For example :
```sol
function sayHiToVitalik(string memory _name) public returns (string memory) {
  // Compares if _name equals "Vitalik". Throws an error and exits if not true.
  // (Side note: Solidity doesn't have native string comparison, so we
  // compare their keccak256 hashes to see if the strings are equal)
  require(keccak256(abi.encodePacked(_name)) == keccak256(abi.encodePacked("Vitalik")));
  // If it's true, proceed with the function:
  return "Hi!";
}
```
Function called with "Vitalik", it will return "Hi!". If you call it with any 
other input then it would throw an error during execution. Thus, require is 
quite useful for verifying conditions that must be true before running a function.

#### Inheritance

Take the example of :
```sol
contract Doge {
  function catchphrase() public returns (string memory) {
    return "So Wow CryptoDoge";
  }
}

contract BabyDoge is Doge {
  function anotherCatchphrase() public returns (string memory) {
    return "Such Moon BabyDoge";
  }
}
```
`BabyDoge` inherits from Doge. That means if you compile and deploy BabyDoge, it will 
have access to both `catchphrase()` and `anotherCatchphrase()` and any other public 
functions that we define on Doge.

#### Imports

When you have multiple files and you want to import one file into another, Solidity
uses the `import` keyword:
```sol
// Imports are made based on relative path
import './someothercontract.sol';

contract new Contract is SomeOtherContract {

}
```

#### Storage vs Memory (Data Location)

In Solidity, there are two locations you can store variables - in `storage` and
in `memory`.

Storage refers to variables stored permanently on the blockchain. Memory variables
are temporary, and are erased between external function calls to your contract. 
Think of it like your computer's hard disk vs RAM.

- State variables (variables outside functions) are by default storage and are 
written permanently to the blockchain, while variables declared inside functions 
are memory and will disappear when the function call ends.

However, there are times when you do need to use these keywords, namely when dealing
with structs and arrays within functions

```solidity
contract SandwichFactory{
    struct Sandwich{
        string name;
        string status;
    }
    
    Sandwich[] sandwiches; 

    function eatSandwich(uint _index) public {
       // Sandwich mySandwich = sandwiches[_index];

        // ^ Seems pretty straightforward, but solidity will give you a warning
        // telling you that you should explicitly declare `storage` or `memory` here.

        // So instead, you should declare with the `storage` keyword, like:
        Sandwich storage mySandwich = sandwiches[_index];
        // ...in which case `mySandwich` is a pointer to `sandwiches[_index]`
        // in storage, and...
        mySandwich.status = "Eaten!";
        // ...this will permanently change `sandwiches[_index]` on the blockchain.

        // If you just want a copy, you can use `memory`:
        Sandwich memory anotherSandwich = sandwiches[_index + 1];
        // ...in which case `anotherSandwich` will simply be a copy of the
        // data in memory, and...
        anotherSandwich.status = "Eaten!";
        // ...will just modify the temporary variable and have no effect
        // on `sandwiches[_index + 1]`. But you can do this:
        sandwiches[_index + 1] = anotherSandwich;
        // ...if you want to copy the changes back into blockchain storage. 
    }
}
```
