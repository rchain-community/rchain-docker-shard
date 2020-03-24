# Run RChain network on a single machine with docker-compose

This repository is a continuation of https://github.com/zsluedem/rchain-docker-cluster.
It's purpose is to deploy RChain shard (plus useful services around it) on local machine using docked-compose. It might be useful for those who wish to test changes to RNode, to get familiar with how network operates and how to configure and start private network. The behaviour is equal to normal network with each node having dedicated machine.

Make sure you give enough resources to docker (check configuration), 6+ CPUs and 8+ GB RAM is preferable.

By default shard of 3 nodes is created, connected with the virtual network `rchain-net`. Each node exposes API ports via docker port mapping. Please see corresponding `.yml` file `ports` section to know exact port numbers.

Initially network state is clean. Once shard is started, nodes will perform genesis ceremony and create genesis block. This takes some time, but after that network can be stopped and started much faster. To clean the state of the network and start with the new genesis block, clear or remove `data` folder.

**_Initial network data_**

* List of public keys of validators bonded in genesis block (bonds file): [./genesis/bonds.txt](./genesis/bonds.txt)

* REV balances in genesis block (wallets file): [./genesis/wallets.txt](./genesis/wallets.txt)

* Private keys and REV addresses for public keys in bonds file (format `PublicKeyHex, PrivateKeyHex, RevAddress`): [./resources/keys.txt](./resources/keys.txt).

* Configuration files for network nodes: [./conf](./conf)

### External Resource

1. [RChain](https://github.com/rchain/rchain)
2. [PyRChain](https://github.com/rchain/pyrchain)

### Dependencies

1. [docker](https://docs.docker.com/install/) >=17.09.0
2. [docker-compose](https://docs.docker.com/compose/install/) >=1.17.0

### Start Network

By default `rchain:rnode/latest` image is used, which is the latest release published. Image can be configured in corresponding .yml files.
To make sure image is up to date with the recent changes published, run `docker pull rchain/rnode:latest` before proceeding.

Start a network of 3 nodes: containers `rnode.bootstrap`, `rnode.validator1` and `rnode.validator2`

    $ docker-compose -f ./shard.yml up

To start only a standalone node (container `rnode.bootstrap`)

    $ docker-compose -f ./standalone.yml up

### [OPTIONAL] Enable automatic block creation

Make sure shard is up and running and all nodes printed `Making a transition to Running state.`, which means Casper instance is ready to accept connections.

    $ docker-compose -f ./proposer.yml up

### [OPTIONAL] Start an observer node

Observer nodes are same as validators but started without private key provided, so they cannot sign and propose blocks. Such instances allow execution of so-called `exploratory deploys` to query the state of the DAG.

    $ docker-compose -f ./read-only.yml up