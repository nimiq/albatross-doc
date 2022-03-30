---
layout: default
title: Block Format
parent: Blockchain
nav_order: 1
---

# Block Format

---

Nimiq 2.0 blockchain has micro and macro blocks. Validators produce micro blocks and propose macro blocks. Both types of blocks have different functions in the blockchain. Micro and macro blocks are divided into three sections: header, body, and justification.

<br/>

**Header**: Contains data of the block itself and commitments to the body and state of the blockchain.

**Body**: The part of the block where transactions or data regarding the staking contract is stored, depending on whether it is a micro or macro block.

**Justification**: Includes necessary data to make the block valid according to the consensus rules and verify the block producer or proposer with its signature.

<br />

**Micro Blocks**
{: .fs-6 }

Micro blocks contain user transactions, and each micro block is produced and signed by a validator according to the [validator selection](/docs/blockchain/slots).

<br />

**Micro header**
{: .fs-5 }

**Version** `u16`: The version number of the block. When the version number changes, it results in a hard fork.

**Block number** `u32`: The number of the block.

**View number** `u32`: The view number of the block. This number increases by one whenever a [view change](/docs/blockchain/view-change) happens, and it resets on every macro block.

**Timestamp** `u64`: The block's timestamp in Unix time with millisecond precision.

**Parent hash** `Blake2bHash`: The hash of the header of the immediately preceding block (either micro or macro).

**Seed** `VrfSeed`: The seed of the block. This seed is the implementation of the [VXEDdSA](https://www.signal.orgdocs/specifications/xeddsa/#vxeddsa) algorithm of the seed of the immediately preceding block (either micro or macro) using the validator key of the block producer.

**Extra data**: Field containing arbitrary data.

**State root** `Blake2bHash`: The root of the [Merkle tree](https://en.wikipedia.org/wiki/Merkle_tree) of the blockchain state. It acts as a commitment to the state.

**Body root** `Blake2bHash`: The root of the Merkle tree of the body. It acts as a commitment to the body.

**History root** `Blake2bHash`: The root of a Merkle Montain Range over all of the transactions that occurred in the current epoch.

<br />

**Micro body**
{: .fs-5 }

**[Fork proofs](/docs/blockchain/fork-proofs)**: This contains the fork proofs of this block. This field might be empty since forks don't occur in every block.

**[Transactions](/docs/accounts-and-transactions/transactions)**: Contains all the transactions of the block. This field might be empty since it is possible to produce blocks without any transactions.

<br />

**Micro justification**
{: .fs-5 }

**Signature:** The signature of the block producer.

**View change proofs:** Consists of the aggregated signatures of a view change message. It might be empty since a view change might not occur in any block.

<br />

Note: `u16`, `u32`, and `u64` refer to the unsigned integer type. `Blake2bHash` refers to the algorithm used for the hash function.

<br />

<p align="center">
  <img src="https://i.postimg.cc/Ghd4SpVY/microblock-drawio.png"/>
</p>

<br />

**Macro Blocks**
{: .fs-6 }

There are two types of macro blocks: election and checkpoint. A new validator list is elected in every election macro block, and the [staking contract](/docs/staking-contract/staking-contract) is updated accordingly. The checkpoint macro blocks serves to reduce the syncing time for new nodes. Macro blocks are produced with [Tendermint](/docs/blockchain/tendermint), where a random validator is chosen to propose the new macro block. User transactions are not included in macro blocks.

<br />

**Macro header**
{: .fs-5 }

**Version** `u16`: The version number of the block. When the version number changes, it results in a hard fork.

**Block number** `u32`: The number of the block.

**View number** `u32`: The view number of the block. This number increases by one whenever a view change happens, and it resets on every macro block.

**Timestamp** `u64`: The block's timestamp in Unix time with millisecond precision.

**Parent hash** `Blake2bHash`: The hash of the header of the immediately preceding block (either micro or macro).

**Parent election hash** `Blake2bHash`: The hash of the header of the preceding election macro block.

**Seed** `VrfSeed`: The seed of the block. This seed is the implementation of the [VXEDdSA](https://www.signal.orgdocs/specifications/xeddsa/#vxeddsa) algorithm of the seed of the immediately preceding block (either micro or macro) using the validator key of the block proposer.

**Extra data**: Field containing arbitrary data.

**State root** `Blake2bHash`: The root of the Merkle tree of the blockchain state. It acts as a commitment to the state.

**Body root** `Blake2bHash`: The root of the Merkle tree of the body. It acts as a commitment to the body.

**History root** `Blake2bHash`: The root of a Merkle Montain Range over all of the transactions that occurred in the current epoch.

<br />

Note: `u16`, `u32`, and `u64` refer to the unsigned integer type. `Blake2bHash` refers to the algorithm used for the hash function.

<br />

**Macro body**
{: .fs-5 }

**Validators**: Contains all the information regarding the next validator list. It includes their validator public key, reward address, and validator slots.

**Public key tree root**: The root of a special Merkle tree over the validator's public keys. It is used in the nano-sync.

**Lost reward set**: It represents which validator slots had their reward slashed when the block was produced. It is used for [reward](/docs/rewards-and-supply/rewards) distribution.

**Disabled set**: It represents which validator slots aren't allowed to produce micro blocks or propose macro blocks when the block was produced. It is used for reward distribution as well.

<br />

**Macro justification**
{: .fs-5 }

**Tendermint proof**: Consists of the aggregated signatures of all validators who have signed the block proposal.

<br />

The following figure demonstrates the anatomy of a macro block and how two macro blocks connect.

<br />

<p align="center">
  <img src="https://i.postimg.cc/BvcDJzFX/macroblock-drawio.png"/>
</p>

<br />

Note that several micro blocks are between an election macro block and a checkpoint macro block. Plus, all the macro blocks are connected to the parent election macro block.

<br />

The following figure demonstrates how the connection between a macro block and a micro block is created.

<br />

<p align="center">
  <img src="https://i.postimg.cc/sxS3RkBL/micro-and-macroblock-drawio.png"/>
</p>


Every block, macro or micro, is connected to the previous one by the parent hash and the random seed.

<br />

**Blockchain format**
{: .fs-6 }

Nimiq 2.0 blockchain is divided into batches and epochs:

**Batch**: The interval between two macro blocks. A batch consists of several micro blocks, closing on a macro block.

**Epoch**: The interval between two election macro blocks marks an epoch. It starts with the first micro block after an election macro block, and it ends at an election macro block, including multiple micro blocks and checkpoint macro blocks in between.

<br />

The following figure illustrates how Nimiq 2.0 blockchain is divided.

<br />

<p align="center">
  <img src="https://i.postimg.cc/C1RFcBwS/epoch-and-batches-drawio.png"/>
</p>

<br />

Nimiq 2.0 starts in epoch 0, with an election macro block, also known as the genesis block. The genesis block results from a hard fork on the Nimiq 1.0 blockchain. It is a hard-coded block, meaning that it wasn’t produced by validators and is part of the source code, unlike the rest of the blocks in the blockchain, which are produced and proposed by validators.

<br />

The genesis block has the same structure as an election macro block, with few exceptions:

- The initial random seed is randomly generated by employing a random number generator.
- Both parent election hash and parent hash fields are empty since there isn't any block behind the genesis block.
