# Fallback

## How to beat this level?

You will beat this level if

1. you claim ownership of the contract
2. you reduce its balance to 0

### A bit of Solidity theory on fallback and receive functions

#### What are fallback and recieved functions used for?

Fallback and recieve functions are used for instances where a contract is called without specifying which existing functions will be invoked.

```
// This line sends 10 eth. The send function does not have a parameter to handle calls to specific functions. it's execution relies on the implementation of a fallback or recieve function to control how the eth will be handled
address.send(10);
// This line transfers 10 eth. The transfer function does not have a parameter to handle calls to specific functions. it's execution relies on the implementation of a fallback or recieve function to control how the eth will be handled
address.transfer(10);
// This line sets a value of 10 eth to be sent, but it does not specify which function will handle the funds - it's blank. It's execution relies on the implementation of a fallback or recieve function to control how the eth will be handled.
address.call{value: 10}("");
// This line sets a value of 10 eth to be sent, but does not specify an existing functions. it's execution relies on the implementation of an existing fallback or recieve function to control how the eth will be handled.
address.call{value: 10}(abi.EncodeWithSignature("functionDoesNotExsit()"));
// In Solidity, the call function can be trigger with just parenthesis and not curly braces - address.call(bytes memory msg). Truthfully, you should only be adding curly braces alongside the call function call when you want to specify gas or ether that you want to send to a contract. The call function expects a single bytes memory parameter. The functions abi.encode, abi.encodePacked, abi.encodeWithSelector and abi.encodeWithSignature can be used to encode. structured data.
address.call(abi.EncodeWithSignature(""))
```

#### What makes the fallback and receive functions special compared to other functions in solidity?

The fallback and receive functions are special because solidity reserves the keywords, "fallback" and "receive" for the creation of these functions. Ironically, these functions cannot be called explicity programmatically, but rather via the EVM.
```
// defining receive function
receive() external payable{
//logic
}
// defining fallback function
fallback() external{
// logic
}
---
//bad code because attempting explicit external function call. Fallback needs to be implicity invoked by EVM when a contract is called without specifying an existing function.
fallback()

//bad code because attempting explicit external function call. Receive needs to be implicity invoked by EVM when a contract is called without specifying an existing function.
receive()
```
Smart contracts can implement at most one fallback and one receive. The EVM will call either of them in instances where calls to a contract are made without specifying an existing function. A contract is a collection of fields and functions and the caller should specify an existing function or else the receive or fallback will be called. 
``` solidity
contract Foo {
event Type(String color);
function apple() public return() {
emit Type("red");
}

receive() payable external {
msg.sender
}

fallback() payable external {
msg.sender
}
```

``` solidity
Foo foo = new Foo();
Address address = (Address)foo;
// Calling no function but sending eth will trigger receive.
address.call{value:32}("");
// Calling a function that does not exist but sends eth will trigger fallback
address.call{value:32}(abi.encodeWithSignature("pear()"));
```
Instead of raising an exception and stopping execution, fallback and receive functions act as a last measure in providing an alternate execution route.

#### How do the fallback and receive help handle instances where a contract's function is incorrectly called?
An outside contract or EOA can either send ether or call a function to trigger execution of a target contract. If ether is being sent to a contract, the msg.data should specify a payable function that will handle the new funds. **The receive function was added to solidity to handle instances specific to a contract receiving ether** when msg.data is empty. In those cases, as long as the receive function is payable, it will receive the ether and can handle execution. If no receive function exists, then the EVM will default to a payable fallback function if it exists. If the msg.data is not empty and does not contain the name of a valid contract function, the EVM will trigger the fallback() as well. If a contract is not "receiving" ether and msg.data is empty or calls a function that doesn't exist, then the fallback function will be called.

#### How is the EVM able to access the fallback and receive functions?
The EVM is able to access the fallback and receive functions by way of the **external** keyword. Adding external scope visability the function definitions grants the EVM access.


#### Whats a easy way to rember the functionally of fallback adn receive?

Recieve function "recieves" ether when msg.data is emmpty

Fallback is the fallback for when msg.data is empty and the contract hasn't implemented a recieve. It is also the fallback for when msg.data has a incorrect function named.

When another contract or Externally Owner Address (EOA) calls the func
- talk about different things that make these functions like payable, external, no actual function name
- talk about different ways a call can be made to fall back (like via call function or send or transfer)
- gas
- explain painpoints of fall back and why receive was added to solidity
- explain why its good to use contact functions that use the abi

#### Basic


### ELIF
Code is a list of instructions. If you interact with the list without specifying a existing instruction, then the default instructions (receive and fallback) will be followed.
