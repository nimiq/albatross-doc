---
layout: default
title: Glossary
nav_order: 16
---

**Account**
{: .fs-5 }

An account consists of a unique address and the respective balance. There are different types of accounts: basic accounts, vesting accounts, HTLC accounts, and staking accounts. Each account has a corresponding key.

**Active validator**
{: .fs-5 }

A validator eligible for selection to the validator slot list. A node must stake its coins into the staking contract to become an active validator.

**Active validator set**
{: .fs-5 }

All the active validators that are in the staking contract.

**Address**
{: .fs-5 }

The identifier of an account.

**Aggregated signature**
{: .fs-5 }

When multiple elected validators sign a transaction, the signatures are aggregated into a single signature to condense the amount of data of the multiple signatures.

**Albatross**
{: .fs-5 }

Nimiq proof-of-stake algorithm. It is an algorithm inspired by the BFT protocol, where nodes can reach a consensus even if some nodes fail to respond. Albatross consists of different types of blocks, validators, and a particular chain selection algorithm.

**BLS signature**
{: .fs-5 }

Signature scheme that uses both bilinear pairing and elliptic curve cryptography, and it has numerous features. Albatross uses the BLS signature scheme for signature aggregation, combining *n* signatures into a single one. Validators create a public key and a private key, and each validator uses its private key to generate a signature. Then, all signatures are aggregated into a single one by summing all up. Therefore, we obtain a single signature, saving a substantial amount of space.

**Batch**
{: .fs-5 }

The interval between two macro blocks, election or checkpoint macro blocks. A batch consists of several micro blocks, closing on a macro block.

**Block height**
{: .fs-5 }

The number of the current block in the blockchain relative to the genesis block. The block with a block height of 9 is the ninth block after the genesis block.

**Body**
{: .fs-5 }

Part of the structure of a block. In a micro block, the body contains the current block transactions and fork proofs. The body of a micro block may be empty as there may haven't occurred transactions or fork proofs. In a macro block, data such as the punishment sets and information of the elected validators are stored in the body.

**Byzantine fault-tolerant algorithm**
{: .fs-5 }

A consensus algorithm that reaches an agreement between elected validators despite the presence of malicious validators. Assuming there are 3*f*+1 elected validators, being at most *f* malicious, this algorithm enables consensus between elected validators. Rational validators wouldn't tamper with the blockchain, as they would get punished.

**Checkpoint block**
{: .fs-5 }

Type of macro block produced with Tendermint. These type of blocks mark the end of a batch and maintain the same validators slot list, elected at the election macro block.

**Coinbase**
{: .fs-5 }

Given the supply formula, the coinbase is the amount of new coins printed at the end of a batch.

**Compressed signature**
{: .fs-5 }

A signature condensed in size to reduce the amount of data of a signature.

**Consensus algorithm**
{: .fs-5 }

Blockchains rely on a consensus algorithm to reach an agreement between nodes. Albatross is a consensus algorithm where nodes can agree on the state of the blockchain even if some nodes fail to respond in time or are offline. Hence, every block is produced through a consensus among elected validators.

**Delay**
{: .fs-5 }

When an elected validator doesn't produce a micro block or propose a macro block at the expected time, it delays the production of blocks. If an elected validator delays the block production, it gets a view change on its view slot.

**Disabled**
{: .fs-5 }

When an elected validator gets disabled, its view slot gets prohibited from producing and proposing blocks until it sends an unparking transaction. Otherwise, it gets cleared from the disabled set at the end of the epoch. The view slot also gets its rewards slashed at the end of the next batch since it couldn't produce any blocks.

**Double-spend attack**
{: .fs-5 }

It occurs when a malicious validator produces two micro blocks at the same block height and with the same view number, creating a fork in the blockchain. A malicious validator creates two different transactions, sending coins to two separate addresses.

**Elected validator**
{: .fs-5 }

An active validator selected to participate in the next epoch becomes an elected validator. The higher the stake it deposits into the staking contract, the higher is the probability of being selected to be an elected validator. Once elected, it can start to produce, propose and sign blocks. 

**Election block**
{: .fs-5 }

Type of macro block poduced with Tendermint. A new validator slot list is elected from the active validator set in the election macro block.

**Epoch**
{: .fs-5 }

The time between two election macro blocks mark an epoch. It starts with the first micro block after an election macro block, and it ends at an election macro block, including multiple micro blocks and checkpoint macro blocks in between.

**Fork**
{: .fs-5 }

Split in the blockchain produced by a malicious validator that may attempt a double-spend attack.

**Genesis block**
{: .fs-5 }

The designation of the first block in the blockchain. Also known as block 0. It is coded by developers, and elected validators produce the following blocks. It also determines the initial state of the blockchain.

**Header**
{: .fs-5 }

Part of the structure of a block. It contains general data of the block, such as the block number, the timestamp, and the version of the block. Includes also some information necessary to the consensus and commitments to the rest of the block. It connects the whole block together and also connects the current block to the previous one.

**Inactive validator**
{: .fs-5 }

A validator that isn't eligible to produce, propose and sign blocks. Despite being in the staking contract, an inactive validator can't be selected to the following validator slot list. It can remove itself from being inactivated by sending a reactivate transaction to the network.

**Justification**
{: .fs-5 }

Part of the structure of a block. In micro blocks, the justification contains the information and signature of the elected validator who produced the block, and it may include view change proofs if any occurred. In macro blocks, the justification consists of a round of signatures produced through the Tendermint protocol.

**Lost reward**
{: .fs-5 }

When an elected validator gets punished, it is added to the lost reward set, meaning that its validator slot won't receive the rewards of the current batch at the end of the next batch.

**Luna**
{: .fs-5 }

The smallest unit of NIM.

**Macro block**
{: .fs-5 }

There are two types of macro blocks: checkpoint macro blocks and election macro blocks. A checkpoint block marks the end of a batch, and the election macro block marks the end of an epoch.

**Malicious validator**
{: .fs-5 }

An elected validator that misbehaves arbitrarily, attempting to disrupt the blockchain.

**Mempool**
{: .fs-5 }

A database where transactions are kept on hold until a validator includes them in the blockchain. A transaction is confirmed once a validator includes it in a micro block.

**Micro block**
{: .fs-5 }

A block produced by an elected validator using its view slots. These blocks contain the blockchain transactions.

**NIM**
{: .fs-5 }

Nimiq's transferable token serves as a form of currency. Additionally, it is used to pay transaction fees as well as staking and voting.

**Parked validator**
{: .fs-5 }

Once an elected validator misbehaves, it gets added to the parked set. It is immediately added to the set, however, it only takes effect at the beginning of the next batch. The parked validator gets inactivated at the end of the epoch unless it sends an unparking transaction.

**Proof-of-stake**
{: .fs-5 }

A blockchain model where nodes put their tokens as a deposit and are able to validate transactions in the blockchain. Nodes are elected proportionally by their stake. A higher deposit increases the probability of a node being selected as a validator.

**Punishment**
{: .fs-5 }

An elected validator who misbehaved as it delayed the block production or forked the blockchain gets punished. It results in the elected validator being added to the punishment sets, becoming unable to produce or propose blocks, and receiving its rewards of the batch where it misbehaved slashed at the end of the next batch.

**Random seed**
{: .fs-5 }

A seed is present in every block. Random seeds are used in the validator selection in order to create a random and secure value. The initial random seed is generated at the genesis block, and the subsequent random seeds are generated implementing the VXEDdSA algorithm instantiated as a verifiable random function.

**Signature**
{: .fs-5 }

Also referred to as a digital signature, it authenticates a message by comprising three functions: generate, sign and verify. Taking a node's private key and a particular message, it is generated a signature. The signature is then verified given the signature and the node's public key.

**Staker**
{: .fs-5 }

Any node that doesn’t have the time or resources to be a validator can propose itself to be a staker. A staker delegates its coins to a validator, which validates blocks on behalf of it.

**Staking**
{: .fs-5 }

To lock a deposit in the staking contract. Either a validator sends its deposit to the staking contract, or a staker does.

**Staking contract**
{: .fs-5 }

A special contract that deals with functions regarding validators, stakers, and staking. It gathers all the contract data in a single location and keeps track of the staking balance, validators list, punishments, and rewards.

**Supply**
{: .fs-5 }

Based on the supply formula, the supply is the number of coins in the blockchain at any given moment. Nimiq 2.0 has a maximum supply of 21 Billion NIM.

**Tendermint**
{: .fs-5 }

Protocol used in Albatross to produce macro blocks. An elected validator proposes a macro block, and the rest of the validator list agrees on the macro block proposal. Validators agree on data regarding the staking contract, such as the punishment sets, rewards distribution, and a new validator list in the case of election macro block.

**Transaction**
{: .fs-5 }

Activity or request recorded in the blockchain that alters its state.

**Transaction fees**
{: .fs-5 }

A small fee charged in transactions that occurred in the blockchain. The transaction fees of a batch are calculated at the end of the batch and divided among the elected validators.

**Validator slot**
{: .fs-5 }

An elected validator can use its validator slots to sign macro blocks and sign view change messages. When an elected validator misbehaves, it gets its validator slot punished. Elected validators also receive their rewards by validator slot.

**View change**
{: .fs-5 }

It occurs when an elected validator delays producing or proposing a block. A new view slot is selected from the view slot list, and the view number increases by one.

**View number**
{: .fs-5 }

A number that keeps track of the view slot at the current block. Its number increases by one each time a view change occurs.

**View slot**
{: .fs-5 }

What allows elected validators to produce a micro block or propose a macro block at a specific block height and specific view number.