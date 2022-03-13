In Albatross, an elected validator that misbehaves gets punished. There are two types of misbehavior:

- Forking the chain, which results in getting punished once a [fork proof]() is submitted.
- Delaying the block production, which results in a [view change]() on the slot owner, thus getting punished.

<br/>

For these types of misbehavior, validators are punished in three modes:

- The misbehaving slot gets its [rewards]() of the batch burned at the end of the next batch. This gives enough time to other validators to submit fork proofs. The burned rewards are sent to the burn address: NQ07 0000 0000 0000 0000 0000 0000 0000 0000.
- The misbehaving slot gets banned from view slot selection, meaning that it can't produce micro blocks or propose macro blocks. Note that it can still sign macro blocks. This only takes effect at the beginning of the next batch.
- The validator gets punished by getting inactivated at the end of the epoch. This means it won't be eligible for validator slot selection in the next epoch.

<br/>

The goal of these punishments in Albatross is to prevent validators from delaying the blockchain by continuous forks or constant view changes, not for validators to lose their stake.

<br/>

Once a validator gets a view change or a fork proof, it is immediately punished. The protocol handles the punishments in three forms:

- Lost reward set: Determines which slots get their reward burned at the end of the next batch. The misbehaving slot is automatically cleared from it in the next batch.
- Disabled set: Dictates which slots can't produce blocks in the next batch. A view slot added to the disabled set also gets its rewards burned since it did not produce any blocks. The disabled set gets cleared at the end of the epoch.
- Parked set: Related to the validator and not exclusively to the misbehaving slot. Once a validator gets parked, it will be inactivated at the end of the epoch. This set gets cleared at the end of the epoch.

<br/>

These sets are in the staking contract and when a validator misbehaves, it gets added to these sets. Note that punishments in forks and delays are the same, but validators are added to these three sets in the staking contract at different times.

Once a view change occurs, the validator is immediately punished. When a fork happens, the malicious validator is punished when a validator submits a fork proof in the blockchain.

Hence, when a validator gets punished, it does not have any immediate implications besides being instantly added to the punishments sets.

<br/>

After being added to these three sets, the validator can send an unparking transaction to the network, preventing it from being inactivated at the end of the epoch. The validator is immediately removed from the disabled set and parked set by sending this transaction. However, it only gets cleared from the lost reward set at the end of the batch since this set gets automatically cleared at every batch.

<br/>

When a validator gets inactivated, it remains in the staking contract, but it can't be selected to the next validator slot list. The end of an epoch sets a new validator slot list, being imperative that validators send the unparking transaction before the end of it.

<br/>

In the following figure, we explain three different scenarios where validators have misbehaved and how punishments are addressed:

<br/>

<p align="center">
  <img src="https://i.postimg.cc/4NNLPRzP/punishments-drawio.png"/>
</p>

*Figure 1*

<br/>

1. A malicious validator forked the chain at micro block A. Then, a validator submitted a fork proof at micro block B, and the malicious validator sent an unparking transaction at micro block C. Note that this occurrence happened during batch 1. Here is how the protocol handled the punishment:

- After the fork at micro block A, a validator submitted a fork proof at macro block B. The misbehaving slot is automatically added, at micro block B, to the lost reward set, disabled set, and the validator got parked. It is still able to produce blocks until the end of batch 1.
- Once it has sent an unparking transaction at micro block C, it got automatically cleared from the disabled set and parked set and became able again to produce blocks in the next batch.
- At macro block D, the entire lost reward set is cleared.
- In batch 2, it started to produce blocks, but it won't receive the rewards at the macro block H since it was in the lost reward set at batch 1.

<br/>

2. A validator got a view change at micro block B, batch 1. This validator sent an unparking transaction at the micro block E, batch 2. Here is how the punishment proceeds:

- After the validator got a view change at micro block B, the slot was added to the lost reward set, disabled set, and the validator got parked.
- Once the new batch began - batch 2 - the slot got cleared from the lost reward set. But it won't produce blocks in batch 2 since it only got unparked during this batch, and it remains in the disabled set at macro block D.
- The validator has sent an unparking transaction at the micro block E, and at that moment, it got cleared from the disabled set and parked set.
- The validator that got the view change won't receive the rewards of batch 1 at macro block H but will receive the rewards of batch 2 at macro block L.

<br/>

3. A validator misbehaved at micro block B but never sent an unparking transaction.

- After a validator misbehaved at micro block B, it got its slot instantly added to the lost reward set and disabled set, and the validator got parked.
- At block D, the validator got cleared from the lost reward set automatically.
- This validator won't receive the rewards of batch 1 at macro block H, neither none of the rewards of the next batches. Either it can produce blocks from the subsequent batch where it got parked.
- It remained parked until the end of the epoch at macro block L, getting inactivated.
