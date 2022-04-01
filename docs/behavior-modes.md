---
layout: default
title: Behavior modes
nav_order: 3
---

# Behavior modes

---

Albatross has two modes for consensus: optimistic and pessimistic mode. The optimistic mode considers that all validators are honest and won't misbehave. But, the pessimistic mode considers having misbehaving validators.

<br/>

**Optimistic mode**
{: .fs-5 }

Considering all validators are rational and don’t attempt to tamper with the blockchain, we demonstrate how the validators would perform to produce and propose blocks:

1. In every new epoch, a new validator list is selected randomly. Validators are chosen proportionally to their stake. The higher the stake, the higher the probability of owning more validator and view [slots](/albatross-doc/docs/blockchain/slots).
2. Using a [VRF seed](/albatross-doc/docs/vrf) present in every block, a validator is chosen to produce the next micro block. The validator will use its view slot to produce it.
3. The validator chooses which transactions to include in the micro block and produces a new VRF seed that will be used to select the next validator view slot.
4. This validator includes the transactions, updates the current state, adds the new VRF seed in the micro block, signs, and broadcasts it.
5. This process repeats in every micro block produced until the end of the batch, where a macro block is proposed, marking the end of the batch or epoch.
6. The macro block leader is selected with the VRF seed present in the previous micro block.
7. The macro block leader proposes a new macro block that doesn’t include any user transactions. If the macro block marks the end of a batch, the validators list remains the same. If the macro block marks the end of an epoch, a new validator list is selected with a VRF seed.
8. To propose a macro block, the block leader and the rest of the validators list must sign the proposal and broadcast it.

<br/>

**Pessimist mode**
{: .fs-5 }

We can also anticipate that some validators can maliciously behave. Validators that act maliciously get punished. Nimiq 2.0 blockchain is inspired by the BFT algorithm, so it tolerates up to one-third of the validators misbehaving. Even considering this case, we can still achieve a decent block production performance.

<br/>

**Micro blocks**: Each micro block is produced by a selected validator. Malicious validators can tamper with the blockchain by:
  - Attempting to fork the chain. In this case, as soon as a rational validator notices the fork, it can submit a [fork proof](/albatross-doc/docs/blockchain/fork-proofs). The malicious validator is punished once this fork proof is sent to the network.
  - Delaying the micro block production. Thereon, a rational validator can send a [view change](/albatross-doc/docs/blockchain/view-change) message, and a new validator is elected to produce the micro block. The delayed validator is then punished.
  - Producing an invalid micro block. Given this case, the rest of the validator list can ignore the invalid micro block.


In either case, the malicious validators are punished according to the [punishment](/albatross-doc/docs/staking-contract/punishments) rules of Albatross.

<br/>

**Macro blocks**: Macro blocks have finality, meaning that they are forkless. Yet, the elected leader can fail with the macro block proposal. There are two ways to attempt to tamper with macro blocks:

  - Failing to make a macro block proposal. If a validator doesn’t propose a macro block in the expected time, [Tendermint](/albatross-doc/docs/blockchain/tendermint) has its procedure to elect a new macro block leader.
  - Creating an invalid proposal. Tendermint ignores invalid proposals, therefore, a new validator is elected as the macro block leader.

<br />

[Back to top](#)