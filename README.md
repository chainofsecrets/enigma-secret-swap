# enigma-secret-swap

Documentation for the test planning and implementation for the ENG to SCRT Burn! proposal created by SecretNodes.org: https://ipfs.io/ipfs/QmUvhWUYW1jpqjZSJjRKUB5y1RixuuvaD27Vbtgbvf1Kjm/Burn_ENG_for_SCRT-v1.pdf

Information included:
- Resources
- Pre-conditions
- Test Plans (Dev, Testnet, Functional and Stress)
- Test Results

## Resources

- [Enigma Swap - Multisig Setup Proposal](https://hackmd.io/AY1XxpRsQey1E-qB3iSyVg)

Written by _ScotchFinance_, an Enigma Blockchain validator, this document outlines the components and workflow for the setup and implementation for the ENG to SCRT swap process.


- [ENG to SCRT Unidirectional Swap Tooling](https://github.com/scotchfinance/scrt-swap)

Also, provided by _ScotchFinance_, this repo contains the UI front-end (for submitting user's swap request) and the code and contract responsible for coordinating the leader and operator actions resulting in the `Burn` request that's emitted for a valid swap request.

The token swap Leader and Operators are elected through an Enigma Blockchain Governance proposal prior, as outlined in the doc above (_Enigma Swap - Multisig Setup Proposal_). The proposal will outline the election of N operators and 1 leader, where the operators wait for a `Burn` event, process the transaction (adding their signature), and storing the transaction in a database. The Leader waits for the `Burn` event as well and creates an unsigned mutisig transaction and stores that in the database. When a `Burn` transaction in the database has M-of-N signatures, the Leader then creates the token swap transaction, stores in the database, and broadcasts the transaction to the validator nodes on the chain.

The `MultisigApproveAddress` used by the Leader is created via the proposal mentioned above.


- [Enigmacli Multisig Transactions](https://github.com/enigmampc/EnigmaBlockchain/blob/master/docs/enigmacli.md#multisig-transactions)

Some information on creating and using Multisig transactions on the Enigma Blockchain.


- [Enigma Blockchain Tokenswap](https://github.com/Cashmaney/enigmachain/tree/master/x/tokenswap)

Created by another Enigma Blockchain validator, _Cashmaney_, this is the repo that contains the actual SCRT minting code and is invoked by the Leader (above).

## Pre-conditions

### Governance Proposals

- [ ] Proposal to identify Leader and N Operators (N to be determined)

- [ ] Proposal to approve the Leader's `MultisigApproveAddress` with information including all involved addresses and method of creation

- [ ] Proposal to set `MintingEnabled` to _true_ to turn on the actual token swap module.

### To Be Determined

- [ ] There's a reference to a _genesis_ file in the `x/tokenswap module`. What do we need to do there?
- [ ] How will the Ethereum `Burn` contract be funded (for gas requirements) prior to enabling the token swap?

## Local Devnet

### Setup

Clone the tokenswap repo:

```
git clone https://github.com/Cashmaney/enigmachain/tree/master/x/tokenswap
```

Build the docker container and run the local Enigma Blockchain:
```
docker build -f .\Dockerfile_build -t enigmachain .
```

Run the container:
```
docker run --name enigmachain -t enigmachain
```

Run a `bash` shell in the `enigmachain` container:
```
docker exec -it enigmachain /bin/bash
```

...

