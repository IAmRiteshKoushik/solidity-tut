## Introduction to Solidity

### Contracts
A `contract` is the fundamental building block of 
Ethereum applications - all variables and functions 
belong to a contract, adn this will be the starting
point of all projects.

```sol
contract HelloWorld{

}
```

All solidity code should start with version pragam which tells the program 
which solidity compiler to use.
```sol
pragma solidity >= 0.5.0 <0.6.0;

contract HelloWorld{

}
```

#### State Variables and Integers
State variables are stored permanently in contract storage. This means that
they are permanently written to the Ethereum blockchain.
```sol
// Data types are defined as follows :
contract Example{
    uint myUnsignedInteger = 100;
}
```
In solidity you have got many types like :
```sol
pragma solidity >= 0.5.0 <0.6.0;

uint8, uint16, uint32 ...
// But in general using uint means
uint256 // 256-bit unsigned integer.
```

#### Math Operations
The following operations happen as follows :
1. Addition: `x + y`
2. Subtraction: `x - y`
3. Multiplication: 'x * y'
4. Division: `x / y`
5. Modulus / remainder: `x % y`
6. Exponent: `x ** y`

#### Struct

```sol
struct Person{
    uint age;
    string name; // Arbitrary-length UTF-8 data
}
```

#### Arrays in Solidity
There are two types of arrays in solidity : fixed and dynamic
```sol
    uint[2] fixedArray; // fixed size array
    string[5] stringArray; // fixed size array
    uint[] dynamicArray; // dynamic array
```
You can also create an array of structs which can be dynamic in nature.
Remember that state variables are stored permanently in the blockchain, so 
a struct like this can be useful in storing structured data inside your
contract (similar to a database).

Public Arrays
```sol
Person[] public people; // create an array of structs of type Person
```

#### Function Declarations
A simple function declaraction in solidity looks like the following :
```sol
function eatHamburgers(string memory _name, uint _amount) public {

}
```
Here's what is happening :
1. This function is taking two parameters of type string and uint
2. The function's visibility is set to public
3. 

What is a reference type in solidity ?

There are two ways in which you can pass an argument to a function :
- By value, which means that the Solidity compiler creates a new copy
  of the parameter's value and passes it to your function. This allows
  your function to modify the value without worrying that the value 
  of the initial parameter gets changed.
- By reference, which means that your function is callled with a ... 
  reference to the original variable. Thus, if your function changes the 
  value of the variable it receives then that change reflects on the 
  original variable.

In solidity, it is convention to start variable names in function parameters 
with an underscore (_) in order to differentiate them from global variables.

```sol
function eatHamburgers(string memory _name, uint _amount) public {
    // body of function
}
```

#### Private and Public Functions

In solidity, functions are `public` by default. This means anyone can call 
your contract's function and execute its code. It is a good practice to mark
your functions are `private` by default and then only make `public` the functions 
you want to expose to the world.

```sol
uint[] numbers;

// It is naming convention to start private function names with underscore
function _addToArray(uint _number) private {
    numbers.push(_number)
}
```

#### Working with Structs and Arrays

We have previously seen that you can have struct elements inside an array. 
You can do the insertion as follows : 
```sol
struct Person{
    uint age;
    string name;
}
Person[] public people;

Person ritesh = Person(16, "Ritesh");
people.push(ritesh) // pushing elements to the array
```

#### More on Functions - Return Values
```sol
string greeting = "What's up dog";

function sayHello() public returns (string memory){
    return greeting;
}
```

In solidity, the function declaration contains the type of the return 
value (in this case - string)

*Function Modifiers*
```sol
// View function is only viewing data, not modifying it
function sayHello() public view returns (string memory){}

// Pure function does not even access any data in the app
function _multiply(uint a, uint b) public pure returns (uint) {
    return a * b;
}
```

#### Keccak256 (Build-in Version of SHA3)
Ethereum has the hash function `keccak256` built-in, which is a version of SHA3.
A hash function basically maps an input into a random 256-bit hexadecimal number. A 
slight change in the input will cause a large change in the hash. `keccak256` 
expects a single parameter of type `bytes`. This means that we have to "pack" any 
parameters before calling `keccak256`.

```sol
//6e91ec6b618bb462a4a6ee5aa2cb0e9cf30f7a052bb467b0ba58b8748c00d2e5
keccak256(abi.encodePacked("aaaab"));

//b1f078126895a1424524de5321b339ab00408010b7cf0e6ed451514981e58aa9
keccak256(abi.encodePacked("aaaac"));

// a single character change lead to a huge change
// Needs further exploration ...
```

#### Typecasting
Sometimes you need to convert between data types. Take the following example :
```sol
uint8 a = 5;
uint b = 6;

// This throws an error because a * b returns a uint, not a uint8
uint8 c = a * c
// We have to typecast b as a uint8 to make it work:
uint8 = a * uint8(b)
```

#### Events 
Events are the way for your contract to communicate that something happened 
on the blockchain to your app front-end, which can be 'listening' for certain 
events and take action when they happen.

Example : 
```sol
// declare the event
event IntegersAdded(uint x, uint y, uint result);

function add(uint _x, uint _y) public returns (uint) {
    uint result = _x + _y; 
    // fire an event to let the app know the function was called
    emit IntegersAdded(_x, _y, result);
    return result;
}
```
You app front-end could the listen for the event. A Js implementation would 
look something like :
```js
YouContract.IntegersAdded(function(error, result){
    // do something with result
})
```

Side Note : `array.push()` returns a `uint` which contains the length of the array

#### Web3.js

Ethereum has a Javascript library called Web3.js. Here is some sample code 
for how Web3.js would interact with the deployed contract

```js
// Here's how we would access our contract:
var abi = /* abi generated by the compiler */;
var ZombieFactoryContract = web3.eth.contract(abi)
var contractAddress = /* our contract address on Ethereum after deploying */;
var ZombieFactory = ZombieFactoryContract.at(contractAddress)
// `ZombieFactory` has access to our contract's public functions and events

// some sort of event listener to take the text input:
$("#ourButton").click(function(e) {
  var name = $("#nameInput").val()
  // Call our contract's `createRandomZombie` function:
  ZombieFactory.createRandomZombie(name)
});

// Listen for the `NewZombie` event, and update the UI
var event = ZombieFactory.NewZombie(function(error, result) {
  if (error) return
  generateZombie(result.zombieId, result.name, result.dna)
});

// take the Zombie dna, and update our image
function generateZombie(id, name, dna) {
  let dnaStr = String(dna);
  // pad DNA with leading zeroes if it's less than 16 characters
  while (dnaStr.length < 16)
    dnaStr = "0" + dnaStr;

  let zombieDetails = {
    // first 2 digits make up the head. We have 7 possible heads, so % 7
    // to get a number 0 - 6, then add 1 to make it 1 - 7. Then we have 7
    // image files named "head1.png" through "head7.png" we load based on
    // this number:
    headChoice: dnaStr.substring(0, 2) % 7 + 1,
    // 2nd 2 digits make up the eyes, 11 variations:
    eyeChoice: dnaStr.substring(2, 4) % 11 + 1,
    // 6 variations of shirts:
    shirtChoice: dnaStr.substring(4, 6) % 6 + 1,
    // last 6 digits control color. Updated using CSS filter: hue-rotate
    // which has 360 degrees:
    skinColorChoice: parseInt(dnaStr.substring(6, 8) / 100 * 360),
    eyeColorChoice: parseInt(dnaStr.substring(8, 10) / 100 * 360),
    clothesColorChoice: parseInt(dnaStr.substring(10, 12) / 100 * 360),
    zombieName: name,
    zombieDescription: "A Level 1 CryptoZombie",
  }
  return zombieDetails;
}
```
