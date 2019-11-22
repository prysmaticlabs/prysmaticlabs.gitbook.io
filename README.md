---
description: >-
  This guide outlines the process of installing and initialising the Prysm
  testnet as well as depositing the initial Goerli ETH required to fully
  participate as a validator in the network.
---

# Getting Started with Prysm

#### **Need assistance?**

If you have questions about this documentation, feel free to stop by either our [Discord](https://discord.gg/KSA7rPr) or [Gitter](https://gitter.im/prysmaticlabs/geth-sharding?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge) and a member of the team or our community will be happy to assist you.

## Dependencies

Prysm can be installed either with Docker **\(recommended\)** or using our build tool, Bazel. The below instructions include sections for performing both.

#### **For Docker installations:**

* The latest release of [Docker](https://docs.docker.com/install/)

#### **For Bazel installations:**

* The latest release of [Bazel](https://docs.bazel.build/versions/master/install.html)
* The latest release of `git`
* A modern UNIX operating system \(MacOS included\)

## Installing Prysm

### Build via Docker

1. Ensure you are running the most recent version of Docker by issuing the command:

```text
docker -v
```

    2. To pull the Prysm images, issue the following commands:

```text
docker pull gcr.io/prysmaticlabs/prysm/validator:latest
docker pull gcr.io/prysmaticlabs/prysm/beacon-chain:latest
```

This process will also install any related dependencies.

### Build via Bazel

1. Open a terminal window. Ensure you are running the most recent version of Bazel by issuing the command:

```text
bazel version
```

    2. Clone Prysm's [main repository](https://github.com/prysmaticlabs/prysm) and enter the directory:

```text
git clone https://github.com/prysmaticlabs/prysm
cd prysm
```

    3. Build both the beacon chain node and the validator client:

```text
bazel build //beacon-chain:beacon-chain
bazel build //validator:validator
```

Bazel will automatically pull and install any dependencies as well, including Go and necessary compilers.

## Connecting to the testnet: running a beacon node

This section contains instructions for initialising a beacon node and connecting to the public testnet. To further understand the role that both the beacon node and validator play in Prysm, see [this section of our documentation](https://prysmaticlabs.gitbook.io/prysm/how-prysm-works/overview-technical)

### Running via Docker

#### **Docker on Linux/Mac:**

To start your beacon node, issue the following command:

```text
docker run -v $HOME/prysm-data:/data -p 4000:4000 --name beacon-node \
  gcr.io/prysmaticlabs/prysm/beacon-chain:latest \
  --datadir=/data
```

The beacon node can be halted by either using `Ctrl+c` or with the command:

```text
docker stop beacon-node
```

To restart the beacon node, issue the following command:

```text
docker start -ai beacon-node
```

To delete a corrupted container, issue the following command:

```text
docker rm beacon-node
```

To recreate a deleted container and refresh the chain database, issue the start command with an additional `--clear-db` parameter:

```text
docker run -it -v $HOME/prysm-data:/data -p 4000:4000 --name beacon-node \
  gcr.io/prysmaticlabs/prysm/beacon-chain:latest \
  --datadir=/data \
  --clear-db
```

#### **Docker on Windows:**

1. You will need to share the local drive you wish to mount to to container \(e.g. C:\).
   1. Enter Docker settings \(right click the tray icon\)
   2. Click 'Shared Drives'
   3. Select a drive to share
   4. Click 'Apply'
2. You will next need to create a directory named `/tmp/prysm-data/` within your selected shared Drive. This folder will be used as a local data directory for Beacon Node chain data as well as account and keystore information required by the validator. Docker will **not** create this directory if it does not exist already. For the purposes of these instructions, it is assumed that `C:` is your prior-selected shared Drive.
3. To run the beacon node, issue the following command:

```text
docker run -it -v c:/tmp/prysm-data:/data -p 4000:4000 gcr.io/prysmaticlabs/prysm/beacon-chain:latest --datadir=/data --clear-db
```

### Running via Bazel

To start your Beacon Node with Bazel, issue the following command:

```text
bazel run //beacon-chain -- --clear-db --datadir=/tmp/prysm-data
```

This will sync up the Beacon Node with the latest head block in the network. Note that the beacon node must be **completely synced** before attempting to initialise a validator client, otherwise the validator will not be able to complete the deposit and funds will be lost.

## Staking ETH: Running a validator client

Once your beacon node is up, the chain will be waiting for you to deposit 3.2 Goerli ETH into the Validator Deposit Contract to activate your validator \(discussed in the section below\). First though, you will need to create a validator client to connect to this node in order to stake and participate.

Each validator represents 3.2 Goerli ETH being staked in the system, and it is possible to spin up as many as you desire in order to have more stake in the network.

### Activating your validator: depositing 3.2 Goerli ETH

To begin setting up a validator, follow the instructions for steps 1 and 2 found on [prylabs.net](https://prylabs.net) to optionally use the Goerli test ETH faucet and make a deposit.

It will take a while for the nodes in the network to process a deposit. Once the node is active, the validator will immediately begin performing its responsibilities.

In your validator client, you will be able to frequently see your validator balance as it goes up over time. Note that, should your node ever go offline for a long period, a validator will start gradually losing its deposit until it is removed from the network entirely.

**Congratulations, you are now running Ethereum 2.0 Phase 0!**

## Setting up a local ETH2 development chain

This section outlines the process of setting up Prysm for local interop testing with other Ethereum 2.0 client implementations. See the [INTEROP.md](https://github.com/prysmaticlabs/prysm/blob/master/INTEROP.md) file for advanced configuration options. For more background information on interoperability development, see [this blog post](https://blog.ethereum.org/2019/09/19/eth2-interop-in-review/).

### Installation and dependencies

To begin setting up a local ETH2 development chain, follow the **Bazel** instructions found in the [dependencies](https://github.com/prysmaticlabs/prysm#dependencies) and [installation](https://github.com/prysmaticlabs/prysm#installation) sections respectively.

### Running a local beacon node and validator client

The example below will deterministically generate a beacon genesis state, initiate Prysm with 64 validators and set the genesis time to your local machines current UNIX time.

1. Open up two terminal windows. In the first, issue the command:

```text
bazel run //beacon-chain -- \
--no-genesis-delay \
--bootstrap-node= \
--deposit-contract 0xD775140349E6A5D12524C6ccc3d6A1d4519D4029 \
--clear-db \
--interop-num-validators 64 \
--interop-eth1data-votes
```

    2. Wait a moment for the beacon chain to start. In the other terminal, issue the command:

```text
bazel run //validator -- --interop-num-validators 64
```

This command will kickstart the system with your 64 validators performing their duties accordingly.

## Testing Prysm

To run the unit tests of our system, issue the command:

```text
bazel test //...
```

To run our linter, make sure you have [golangci-lint](https://github.com/golangci/golangci-lint) installed and then issue the command:

```text
golangci-lint run
```

