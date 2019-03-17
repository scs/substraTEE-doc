# substraTEE-doc
documentation for substraTEE, an extension of [parity substrate](https://github.com/paritytech/substrate) allowing to leverage Trusted Execution Environments (TEEs) to provide integrity and confidentiality

# Different ways to leverage TEEs

| use case | substraTEE-client <br>(off-chain stateless) | substraTEE-delegate<br> (off-chain stateful) | substraTEE-node<br> (onchain-stateful) |
|----------|-------------------|----------------|-----------------|
|hardware wallet| :+1: local TEE per user | | :thumbsdown:|
|atomic swaps<br>(cross-chain bridge)| :+1: light node in both chains | :+1: | :thumbsdown: |
|oracle| :+1: | no benefit wrt "-client" | difficult if non-deterministic |
|inheritance notary| :+1: | :+1: | storage expensive |
|confidential transactions| :thumbsdown: | :thumbsdown: doesn't scale? (collisions of state changes) | :+1: [encointer](https://encointer.org) |
|confidential smart contracts |:thumbsdown: | :+1: (Ekiden, PDO, [encointer](https://encointer.org)) | computation time and storage expensive|
| [POET](https://sawtooth.hyperledger.org/docs/core/releases/1.0/architecture/poet.html) consensus | :thumbsdown: | :thumbsdown: | :thumbsdown: |


# substraTEE-client
One flavour of substraTEE is a RPC client for substrate that runs a state transition function (STF) within a TEE (Intel SGX). 

Main feature: trusted hardware custodian of your private keys

![substraTEE-client](./substraTEE-client-overview.svg)

# substraTEE-delegate
Similar to [sawtooth PDO](https://github.com/hyperledger-labs/private-data-objects) or [Ekiden/OasisLabs](https://www.oasislabs.com/)

Delegates can offer their enclave as a service to run confidential smart contracts on. delegates are remote attested on the blockchain. They have to be fed with the most recent state, call and payload. They then update the state that is written back to the chain.

# substraTEE-node
a fork of substrate that has an Executor running in a TEE (Intel SGX)

Main feature: many confidential transactions can be executed with every block
