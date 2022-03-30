---
layout: default
title: Overview
nav_order: 15
---

# Overview

---

Nimiq 2.0 is a proof-of-stake blockchain. Instead of mining blocks and consuming large amounts of energy as in proof-of-work blockchains, Nimiq 2.0 blockchain incentivizes users to become validators and stakers by investing their stake as a deposit. Albatross, Nimiq's proof-of-stake consensus algorithm, allows the network to perform even if some validators fail to respond. The main purpose of a consensus algorithm is to reach an agreement between nodes to ensure the blockchain state is valid and accurate.

<br />

**Block Format**
{: .fs-5 }

This novel blockchain performs with two types of blocks: micro and macro blocks. Micro blocks are produced by validators using their view slots, and the macro blocks are proposed by validators also using their view slots, following the Tendermint protocol. There are two types of macro blocks: checkpoint and election macro blocks. The checkpoint macro block marks the end of a batch, and an election macro block marks the end of an epoch. Thus, the blockchain is divided into batches and epochs. A batch consists of several micro blocks and ends in a checkpoint macro block, while an epoch consists of several batches and ends in a checkpoint macro block. At every checkpoint macro block, a new validator list is selected.

<br />

**Behavior modes**
{: .fs-5 }

Albatross has two behavior modes to reach an agreement between nodes: optimistic mode and pessimistic mode. In the optimistic mode, all validators are honest and don't misbehave. In the pessimistic mode, we assume having misbehaving validators. Misbehaving validators get punished and get added to the punishment sets. In this last mode, the blockchain resumes its normal behavior in three ways:

- Invalid blocks are ignored.
- View changes are reverted once a rational validator sends a view change message.
- Forks are reverted once a rational validator submits a fork proof.

<br />

**Staking Contract**
{: .fs-5 }

Nimiq developed a particular contract to support the new blockchain that handles functions regarding validators and keeps it in a single location - the staking contract. Any user with a balance in Nimiq's network can propose to become a validator or a staker. Validators validate blocks and get rewarded for their work. Stakers can delegate their coins for validators to use their stake.