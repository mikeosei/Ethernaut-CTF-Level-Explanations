## What makes something random?

A random event is an event that the outcome cannot be easily predicted. The resistance of a probabilistic scenario from artificial tampering. You cannot 100% determine the result of a coin flip or the 2050 nba champion. These random events rely on a variety of factors that increase the entropy in each system.

## Why is randomness difficult reproduce on the blockchain?

The blockchain by nature is deterministic - the hash of each block is based on the hash of the previous blocks before it. (If bad actor manipulates the previous block then the current blocks hash will consequently change).This is good for maintaining the integrity of the distributed ledger but this publicness of all code published to the blockchain is bad for using internal blockchain properties a the basis for entropy when trying to acheive randomness. Creating randomness in a computer requires the creation of a randomness factor that varies. For exampe, block.number is a property that some contracts use as a source of randomness. But block.number is public and so if knowing the block.number was the crux of randomness then the events’ outcome will be rendered deterministic.

// Randomness provided by this is predicatable. Use with care!

```
function randomNumber() internal view returns (uint) {
    return uint(blockhash(block.number - 1));
}
```

The belief is this function will generate a random number, but it isn’t so random because if I know the block number is 67890, I can hash it myself and now I have the random number.

In a way, we are able to "frontrun" the execution of a randomize event that uses block.number, by running the exact same code.