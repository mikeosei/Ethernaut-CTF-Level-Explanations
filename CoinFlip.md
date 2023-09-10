## What makes something random?

A random event is an event where the outcome cannot be easily predicted. The other words, the resistance of a probabilistic scenario from artificial tampering. You cannot 100% determine the result of a coin flip. Random events rely on a variety of factors that increase the entropy in each system. In computer science, it's hard to replicate the factors that influence a coin flip such as air drag, force, angle, and coin quality. Furthermore, the coded replication of a probablistic scenerio on a computer inherently includes the step/conditions that will lead to a certain outcome.

## Why is randomness difficult to reproduce on the blockchain?

The blockchain by nature is deterministic - the hash of each block is based on the hash preceeding blocks. So, if bad actor manipulates the previous block then the current blocks' hash will consequently change. This is good for maintaining the integrity of the distributed ledger. However, using internal blockchain properties a the basis for entropy when trying to acheive randomness is bad because the publicness/determinism allows users to simulate probabilistic events and extract results prior to the official event. For example, block.number is a property that some contracts use as a source of randomness. But block.number is public and so if knowing the block.number was the crux of randomness then the events’ outcome will be rendered deterministic. A user can simulate the probablistic event with the public block.number and obtain a results prior to the official event.

// Example of a function generating a random number

```
function randomNumber() internal view returns (uint) {
    return uint(blockhash(block.number - 1));
}
```

The belief is this function will generate a random number, but it isn’t so random because if I can simulate this same function on my local machine for example and now I have the random number before the function is officially ran on the blockchain. In a way, we are able to "frontrun" the execution of a randomize event that uses block.number or other blockchain properties, by running the exact same code.
