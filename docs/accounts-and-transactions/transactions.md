---
layout: default
title: Transactions
parent: Accounts and Transactions
nav_order: 3
---

# Transactions

---

Transactions are the means to communicate with the blockchain. A transaction alters the state of the blockchain, according to the blockchain rules. Transactions are stored in the micro block body. There are four types of [accounts](/albatross-doc/docs/accounts-and-transactions/accounts) in Nimiq's blockchain that allows a user to send and receive transactions.

<br/>

The transaction is sent to the network, and it is kept in the mempool, where all the transactions stay on hold until a validator includes them in the blockchain. The transaction is included once the validator confirms and validates the transaction.

Transactions require the payment of a fee. The sender might not include a fee in its transaction, but the validators can disregard it and exclude it from the blockchain. Fees pay the validator's rewards and serve as an incentive for validators to include the transactions in the blockchain.

Transactions can't have the same sender and recipient except for the staking contract, where it's possible to send a transaction from the staking contract to the staking contract.

<br/>

**Transaction data**
{: .fs-5 }

**Data**: Miscellaneous data. This field is to be filled in with data to be sent, other than tokens. It has several use cases for example for changing data on an account.

**Sender**: The sender's address.

**Sender type**: The type of account of the sender. It can be a basic account, a HTLC contract, a vesting contract, or the staking contract.

**Recipient**: The recipient's address.

**Recipient type**: The type of account of the recipient. It can be a basic account, a HTLC contract, a vesting contract, or the staking contract.

**Value**: Amount of NIM to transfer. This field can be zero since there are transactions without any value. In this case, the data to transact is filled in the “data” field.

**Fee**: Fees for the transaction. The fees are paid by the transaction's sender with the amount of their choice. The fees are part of the rewards, so it's essential to add a fee in the transaction, so the validators can be rewarded for their work.

**Validity start height**: The expiration date for a particular transaction. The elected validator has a window of x blocks from the beginning to upload the transaction. If included after those x blocks, the transaction is disregarded.

**Network ID**: Field where is specified the ID of the network. When the sender sends a transaction, he must include the particular network to where the transaction is sent to.

**Flags**: Flag that signals the type of transaction. There are two types of flags: contract creation and signaling.

<br/>

**Proof**: The information above is then hashed and calculated with the sender’s private key, resulting in the proof for the transaction. This proof authenticates the sender of the transaction.
