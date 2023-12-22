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

