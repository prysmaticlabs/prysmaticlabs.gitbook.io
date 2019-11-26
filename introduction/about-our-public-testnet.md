---
description: >-
  The various features and limitations of the Prysm 'Sapphire' Phase 0 testnet
  release.
---

# On the Public Testnet

## Features of this release

* **The testnet is publicly accessible and NOT a simulation.** We provide a cloud cluster of nodes that participate in consensus, but anyone can join the network and contribute.
* **The testnet is a single client 'Prysm network'.** Unlike Ethereum 1.0, which has Geth and Parity, this network is entirely Prysm-only.
* **Staking of Goerli test ETH is included in the network.** Users can utilise the deposit contract, run a validator client, and participate in consensus to earn rewards or penalties. 
* **Features LibP2P by** [**Protocol Labs**](https://protocol.ai/) for decentralised, peer-to-peer networking of nodes.
* **Implements v0.4 of the official beacon chain specification** created by the Ethereum Research team \(the latest version is v0.8.3\). 

## Limitations of this release

* **The network does not include smart contract or EVM-related functionality.** This is a part of Ethereum 2.0 Phase 2. The current testnet is only tasked with managing a registry of [validators](../glossaries/terminology.md#validator), allowing for Casper [Proof-of-Stake](../glossaries/terminology.md#proof-of-stake-pos) and the advancement of the blockchain. 
* **This is NOT a multi-client network**, though this is the next step most ETH2 teams have in mind. 
* **The testnet uses different configuration parameters than what we’d see on mainnet;** that is, we support fewer shards, a smaller [validator](../glossaries/terminology.md#validator) count, and different constants for the sake of simplicity. 
* **The testnet does not contain beacon chain transfers or withdrawals**, as those will come in later iterations and are not critical for showcasing the core functions of the beacon chain. 
* **The testnet is not optimized for a large amount of validators to join.** That is, we have not yet implemented a super-optimal LMD GHOST [fork-choice rule](../glossaries/terminology.md#fork-choice-rule), among other features, to make the code robust enough for hundreds of thousands of [validators](../glossaries/terminology.md#validator) to join.

