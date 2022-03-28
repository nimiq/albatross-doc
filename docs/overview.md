---
layout: default
title: Overview
nav_order: 2
---

**Nimiq 2.0**
{: .fs-5 }

Nimiq 2.0 is a proof-of-stake blockchain. Instead of mining blocks and consuming large amounts of energy as in proof-of-work blockchains, our blockchain incentivizes users to become validators and stakers by investing their stake as a deposit.

<br/>

**Albatross**
{: .fs-5 }

Albatross is a proof-of-stake consensus algorithm that allows the Nimiq 2.0 network to perform. The main purpose of a consensus algorithm is to reach an agreement between nodes to ensure the blockchain state is valid and accurate. It is a fault-tolerant algorithm, meaning nodes can reach an agreement even if some validators fail to respond.

<br/>

**Blockchain**
{: .fs-5 }

Albatross blockchain has two types of blocks: [micro and macro blocks](/docs/block-format). Micro blocks are produced by validators using their view [slots](/docs/slots). Macro blocks are proposed with [Tendermint](/docs/tendermint) by validators using their view slots.

Also, we have two types of macro blocks: checkpoint and election macro blocks. The checkpoint macro block marks the end of a batch, and an election macro block marks the end of an epoch.

Our blockchain is divided into batches and epochs. A batch consists of several micro blocks and ends in a checkpoint macro block, while an epoch consists of several batches and ends in a checkpoint macro block. At every checkpoint macro block, a new validator list is selected.

<br/>

**Albatross behavior**
{: .fs-5 }

Albatross has two [modes](/docs/behavior-modes) for consensus: optimistic and pessimistic mode. The optimistic mode considers that all validators are honest and won't misbehave. But, the pessimistic mode considers having misbehaving validators. Misbehave validators get punished and are added to the [punishment sets](/docs/punishments). The blockchain resumes the normal behavior in three ways:

- Invalid blocks are ignored.
- View changes are reverted once a rational validator sends a [view change](/docs/view-change) message.
- Forks are reverted once a rational validator submits a [fork proof](/docs/fork-proofs).

<br/>

**Staking contract**
{: .fs-5 }

It's a particular contract that stores the data on validators and stakers in a single location. The [staking contract](/docs/staking-contract) deals with all the functions related to staking. Any user with a balance in Nimiq's network can propose to become a validator or a staker. Validators validate blocks and get rewarded for their work. Stakers can delegate their coins for validators to use their stake.

<br/>

**Accounts and transactions**
{: .fs-5 }

Nimiq supports four types of [accounts](/docs/accounts): basic, HTLC, vesting contract, and staking contract. Each account has a unique address, and users can communicate with the blockchain through [transactions](/docs/transactions).