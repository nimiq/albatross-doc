---
layout: default
title: Blockchain
nav_order: 4
has_children: true
---

# Blockchain

---

Nimiq 2.0 blockchain includes two types of blocks: micro and macro blocks. Validators produce micro blocks using and propose macro blocks using their view slots. Macro blocks are proposed with the Tendermint protocol, with certain variations.
{: .fs-6 .fw-300 }

Albatross assume some validators can misbehave by attempting to fork the chain or delaying the block production. Considering this case, rational validators can submit fork proofs or request a view change on the misbehaving validator.
{: .fs-6 .fw-300 }