Nimiq 2.0 has four types of accounts:

- Basic
- HTLC
- Vesting contract
- Staking contract

Users can operate with their accounts and interact with the blockchain through [transactions](). Each account has a unique address. They are stored in the state tree of the blockchain.

### **Basic account**

With a basic account, users can send and receive NIM and have a balance. The account is controlled with a private key unique to the user. This type of account is similar to a bank account with an address, a balance, and a key. Once a user sends or receives NIM, the balance is updated.

A basic account is created once any user sends NIM to an inexistent address.

### **HTLC - Hashed Timelock Contract**

A HTLC is a conditional payment implemented by a script in the blockchain. It's a smart contract that enables a party to transfer assets to another party without relying on a third one. The contract acts as the escrow party for both sender and recipient.

The structure of the HTLC:

- **Balance**: The balance of the contract at any given time. It can differ from the “total amount” since it’s possible to withdraw partial amounts at a time.
- **Sender**: The sender’s address.
- **Recipient**: The recipient’s address.
- **Hash algorithm**: The algorithm used for the hash function.
- **Hash root**: The hash of the preimage - secret key.
- **Hash count** `u8`: The number of times the preimage is hashed so the recipient can withdraw the funds entirely.
- **Timeout** `u64`: The timeout is decided once the contract is created. Otherwise, the funds can be withdrawn by the sender. This timeout is in Unix time with millisecond precision.
- **Total amount**: Refers to the initial amount decided in the contract creation.

Anyone can create a HTLC whose structural values are static (decided by the contract owner) besides the balance, which changes every time NIM is withdrawn. Also, once the HTLC is created, no NIM can be added to the contract.

Both `u8` and `u64` refer to the unsigned integer type.

There are three different transactions to unlock the funds:

1. **Timeout resolve** - Ater the timeout elapses, the sender can redeem the funds.
2. **Regular transfer** - The recipient can withdraw the funds entirely or partially before the timeout elapses. Supposing that the "hash count" is three, the contract owner can present a hash that was rehashed two times resulting in the "hash root" and then withdraw two-thirds of the funds. He can also present the preimage and withdraw the "total amount" immediately.
3. **Early resolve** - When both sender and recipient sign the transaction the funds can be withdrawn at any time.

Sending this transaction results in a new balance in the HTLC.

**Vesting contract**

The vesting contract allows a user to lock funds for a period of time and unlock them in a predefined timetable. This contract locks the funds of a single user (the contract owner), and it can unlock the funds in predefined portions.

Structure of the vesting contract:

- **Balance**: The balance of the contract at any given time. Whenever the funds are spent, the balance changes.
- **Owner**: The address of the owner of the contract. The contract owner has a corresponding key to sign and withdraw the funds.
- **Start time** `u64`: The time when the vesting schedule starts, decided by the owner. It is in Unix time with millisecond precision.
- **Time step** `u64`: The “step amount” unlocks at each time step. For instance, if the owner decides to unlock the funds every 24 hours (the time step), the “step amount” unlocks.
- **Step amount**: The amount of NIM unlocked in every “time step”.
- **Total amount**: Refers to the initial amount decided in the contract creation.

These values are static, except for the balance, which changes as the funds are withdrawn. The values are decided in the creation of the contract.

`u64` refers to the unsigned integer type.

The contract owner can interact with the contract whenever he desires but can only withdraw the partial amount of NIM decided when the "time step" unlocks. The balance is then updated.

Note that unlocking the funds is a predefined action, and it happens at every “step amount”. Yet withdrawing the funds is an owner’s action. Also, once the vesting contract is created, no NIM can be added to the contract.

**Staking contract**

The staking contract is a Nimiq 2.0 type of contract that allows nodes to be validators and stakers, thus being a part of the consensus. Any node with a wallet and stake in Nimiq's blockchain can propose to be a validator or a staker.

For a detailed explanation about the staking contract, follow this [link]() to read on validators, stakers, and their interactions with the staking contract.
