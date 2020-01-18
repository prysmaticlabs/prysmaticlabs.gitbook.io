---
description: >-
  This section contains information on peer-to-peer networking in the Prysm
  client as well as the thought process behind selecting libp2p for this role.
---

# P2P Networking

As stated, Ethereum 2.0 is a distributed and decentralised peer-to-peer \(P2P\) network. To accomplish this connectivity, Prysm has decided to utilise the popular [libp2p](https://libp2p.io/) project developed by [Protocol Labs](https://protocol.ai/). 

#### Why libp2p?

Libp2p is a full featured and highly maintained next-generation library. By implementing an already existing and proven peer-to-peer system into Prysm, the low level functions of networking across nodes are managed both effectively and securely 'out of the box', while simultaneously freeing human resources and allowing development to focus on the higher level functionality of the client.

For more information and current developments, see the [official Ethereum 2.0 P2P specification](https://github.com/ethereum/eth2.0-specs/blob/dev/specs/phase0/p2p-interface.md).



