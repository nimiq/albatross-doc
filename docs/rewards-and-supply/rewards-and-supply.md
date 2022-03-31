---
layout: default
title: Rewards and Supply
nav_order: 5
has_children: true
---

# Rewards and Supply

---

Validators are rewarded for producing, proposing, and signing blocks. The rewards are distributed equally among the validator slot list at the end of the batch after the one validators participated in validating blocks. This delay in a batch allows validators to submit fork proofs since the staking contract punishes malicious validators by burning their rewards. The amount of the rewards per batch consists of the coinbase, calculated given Nimiq's supply formula and the transaction fees.
{: .fs-5 .fw-300 }