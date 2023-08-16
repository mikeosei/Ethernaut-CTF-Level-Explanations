# Fallback

## How to beat this level?

You will beat this level if

1. you claim ownership of the contract
2. you reduce its balance to 0

### A bit of Solidity theory on fallback and receive functions

#### What makes fallback and receive functions special?

The fallback and receive functions are special because:

- Solidity reserves the keywords, "fallback" and "receive" for the creation of these function. Ironically, these functions cannot be called explicity programmatically, but rather via the EVM. Since the EVM calls these functions, it expects those keyworks and this eliminates the need for the programmer to define custom names. Smart contracts implement fallbacks and receives, and the EVM will call either of them in instances where a contract is incorrectly called. Instances where contracts are called incorrectly are when a EOA or another contract calls a function that does not exist. Instead of raising an exception and stopping execution, fallback and receive functions act as a last measure in providing an alternate execution route.

#### How do the fallback and receive help handle instances where a contract's function is incorrectly called?
An outside contract or EOA can either send ether or call a function to trigger execution of a target contract. If ether is being sent to a contract, the msg.data should specify a payable function that will handle the new funds. **The receive function was added to solidity to handle instances specific to a contract receiving ether** when msg.data is empty. In those cases, as long as the receive function is payable, it will receive the ether and can handle execution. If no receive function exists, then the EVM will default to a payable fallback function if it exists. If the msg.data is not empty and does not contain the name of a valid contract function, the EVM will trigger the fallback() as well. If a contract is not "receiving" ether and msg.data is empty or calls a function that doesn't exist, then the fallback function will be called.

#### How is the EVM able to access the fallback and receive functions?
The EVM is able to access the fallback and receive functions by way of the **external** keyword. Adding external scope visability the function definitions grants the EVM access.


#### Whats a easy way to rember the functionally of fallback adn receive?

Recieve function "recieves" ether when msg.data is emmpty

Fallback is the fallback for when the above function is true but the contract hasn't implemented a recieve. for when msg.data has a incorrect function named

When another contract or Externally Owner Address (EOA) calls the func
- talk about different things that make these functions like payable, external, no actual function name
- talk about different ways a call can be made to fall back (like via call function or send or transfer)
- gas
- explain painpoints of fall back and why receive was added to solidity
- explain why its good to use contact functions that use the abi

#### Basic


### Real World Example
