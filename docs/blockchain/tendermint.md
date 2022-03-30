---
layout: default
title: Tendermint
parent: Blockchain
nav_order: 4
---

# Tendermint

---

Tendermint is a protocol implemented in blockchains that allows consensus among nodes. It has several use cases besides blockchains, but in this document, we explain how our protocol implements Tendermint.

The Tendermint protocol was inspired by the BFT (Byzantine fault-tolerant) consensus algorithm. It is a fault-tolerant protocol, meaning that nodes can reach an agreement even if up to one-third of nodes fail to respond. Blockchains assume that not all validators are permanently online, and some may attempt to disrupt the blockchain.

Validators agree on a new block by gossiping about a block proposal and signing the votes for the block. The block is effectively produced if at least two-thirds of all validators agree on the block, thus signing it.

<br/>

**How does the protocol work?**

In a proof-of-stake blockchain, to produce a new block, validators are selected proportionally by their stake. The validator chosen to produce the block is called the leader, and it has a determined amount of time to propose a block. To produce a block three steps must be concluded: propose, prevote and precommit. Either the block is produced, or a new round of voting starts.

<br/>

**Tendermint in Albatross**
{: .fs-5 }

Albatross implements Tendermint to produce macro blocks, both checkpoint and election. Validators agree on data such as the [punishments](/docs/staking-contract/punishments) sets and the list of validators that will receive the [rewards](/docs/rewards-and-supply/rewards) from the previous batch.

Elected validators use their view [slots](/docs/blockchain/slots) to propose a macro block and their validator slots to sign the block proposal. Each one is selected from the view slot list and validator slot list, respectively. Elected validators with their view slots disabled can't propose macro blocks but can still use their validator slots to sign the proposals.

There are two steps of voting, therefore two rounds of signatures. The signatures are aggregated into a single signature in order to spare data space. Once a macro block is produced, it is added at a new block height. Both rounds of signatures are equivalent to the same proposal since the second round only occurs if the first is terminated. Hence, we only store the second round of signatures, reducing the amount of data in the macro block justification as well.

<br/>

**Implementing Tendermint to produce a macro block**
{: .fs-5 }

Tendermint starts by selecting an elected validator from the view slot list to propose a macro block. It proposes a macro block and gossips it to the network within a time frame. If it doesn't propose a macro block in time, another elected validator is selected to propose a macro block. Elected validators prevote and precommit to a proposal or vote null (_nil_). The algorithm terminates once all the steps are satisfied.

<br/>

Now, we explain the Tendermint steps implemented by Albatross to produce a new macro block, followed by a figure demonstrating each step.

<br/>

1. Leader broadcasts a macro block proposal and:

   1. Elected validators receive a new proposal from the leader.
   2. Elected validators receive a past proposal from a previous leader.
   3. Elected validators didn't receive any proposal in time.

2. Elected validators broadcast their prevote and wait for:

   1. 2𝑓+1 prevote messages for the block.
   2. 2𝑓+1 _nil_ prevote messages for the block.
   3. Less than 2𝑓+1 prevote or _nil_ messages were signed.
   
3. Elected validators broadcast their precommit and wait for:

   1. 2𝑓+1 precommit messages for the block, terminating the algorithm.
   2. Less than 2𝑓+1 _nil_ precommit messages, and a new round starts.

<br/>

<p align="center">
  <img src="https://i.postimg.cc/0jKxGb7F/Tendermint-drawio.png"/>
</p>

<br/>

Each step must be followed in order to produce a macro block. To prevote for the macro block, elected validators must have received a proposal. To precommit for the macro block, elected validators must have signed the prevote step.

A new round starts when the designated leader proposes a macro block but doesn't receive two-thirds of votes for it. The second leader can propose the same macro block as the first did, and it results in a round number increased by one but the same view number. The second leader can also propose a new macro block, but in that case, both round number and view number increase by one.

<br/>

**Variations on the original protocol**
{: .fs-5 }

In order to simplify and reduce the amount of data that running the original protocol would result in, Albatross developed minor variations to implement Tendermint.

<br/>

| TENDERMINT                                                                                                                                                           | ALBATROSS TENDERMINT                                                                                                                                                     |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| All proposals are sent to the validators. The leader can send five proposals to different validators, but validators don't gossip to the network all five proposals. | Our protocol only accepts the first valid proposal from the leader, ignoring the following proposals. The first proposal a validator receives is the effective proposal. |
| The protocol accepts invalid proposals, but validators don't sign invalid proposals.                                                                                 | The protocol immediately ignores invalid proposals. Validators only receive valid proposals.                                                                             |
| Tendermint is constantly listening to messages: proposals, prevotes, and precommits.                                                                                 | Albatross use another part of the protocol to receive votes, and it terminates each step once that part is satisfied.                                                    |
| As soon as 2𝑓+1 precommit messages are received, the algorithm ends Tendermint.                                                                                      | A part of Albatross is always listening for complete blocks. Once that part listens to a complete block, it terminates the Tendermint algorithm.                         |
