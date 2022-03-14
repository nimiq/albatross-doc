When a potential validator becomes an active validator, it can start producing blocks. If the validator successfully produces blocks without forking or [delaying](https://github.com/nimiq/albatross-doc/blob/main/ViewChange.md) the blockchain, it will be rewarded at the macro block of the next batch.

<br/>

During a batch, there is a chance of malicious validators forking the chain or a validator delaying the production of blocks. Consequently, the slots that have misbehaved are punished by getting their rewards burned.

<br/>

Validators can submit a [fork proof](https://github.com/nimiq/albatross-doc/blob/main/ForkProofs.md) until the end of the batch since the rewards are distributed at the end of the next batch. Essentially, there is a delay of one batch. The rewards are calculated from the macro block of the current batch and distributed at the macro block of the next batch. 

<br/>

The rewards consist of the coinbase and the transaction fees. The coinbase is the amount of new coins printed at the end of every batch, and the transaction fees are the sum of all transaction fees from a batch.

<br/>

In the Albatross protocol, both coinbase and transaction fees are variable. Oppositely to Bitcoin, for example, where the coinbase is the same by block produced, decreasing 50% approximately every 4 years. The coinbase in Nimiq 2.0 varies on time since new coins are printed per batch and not by block produced, as it happens in Bitcoin. 

<br/>

To calculate the coinbase, Nimiq's 2.0 has a formula that predicts the supply at any given time, given three parameters:

- Initial supply: the supply that Nimiq 2.0 will start with, denoted by *S₀*
- Initial velocity: a constant parameter which determines the number of NIM created initially by unit of time represented by *V₀*
- Decay: a constant that dictates the percentage of the velocity decrease, denoted by β

<br/>

The Nimiq 2.0 supply formula is the following:

<img src="https://render.githubusercontent.com/render/math?math=S(t)=S_0+\frac{V_0}{\beta}(1-e^{-\beta t})">

<br/>

Additionally, 𝑡 is the time elapsed since the genesis block, and *e* is the exponential function.

<br/>

This formula is coded in the protocol and returns the supply of the coinbase at any given time in seconds, and it is distributed in Lunas (1 NIM = 100 000 Lunas).

Essentially, the coinbase is calculated by subtracting the supply calculated in the blockchain at any given time, from the previous supply, which is the total amount of NIM at the end of the last batch. This is calculated at the exact moment the reward is distributed.

<br/>

For a detailed explanation about the supply formula to calculate the coinbase: [click here](https://github.com/nimiq/albatross-doc/blob/main/SupplyFormula.md).

<br/>

### Reward distribution:

The rewards of a batch are equally divided among the validator slot list. Hence, the reward is equally divided among all the slots. The validator slot list contains 512 view slots, and validator can have one view slot or multiple ones.

When a validator misbehaves, it will get punished and have its rewards burned. If a validator forks the chain or delays the production of blocks, it won't receive the rewards of the batch where it has misbehaved.
