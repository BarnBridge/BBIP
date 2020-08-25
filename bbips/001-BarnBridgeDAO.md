---
bbip: 1
title: BarnBridge DAO
torchbearer: Daniel Luca (@CleanUnicorn)
author: Daniel Luca (@CleanUnicorn)
discussions-to: <URL, usually the pull request>
status: Draft
created: 2020-08-25
---

## Simple summary

The kernel of the BarnBridge platform is a DAO controlled by the community. This describes that DAO.

## Abstract

This smart contract needs to be a governance, also known as a Decentralized Autonomous Organization, referred to as **BarnBridge DAO**. 

The design and architecture of this DAO defines what can be done in the future, thus it needs to be flexible enough to allow for future functionality to be added, updated or removed. An in progress standard known as [EIP-2535 Diamond Standard](https://eips.ethereum.org/EIPS/eip-2535) defines an architecture which can be used to create DAOs with a flexible set of features.

The BarnBridge DAO will become the core component of the BarnBridge Platform because it will be able to make decisions in a decentralized manner that will be able to enforce the best actions for the wellness of the community.

## Background

In order to choose how the architecture should behave and what features this DAO should have, we are looking at the existing DAOs in the Ethereum ecosystem.

The following DAOs (or governance templates) were considered for inspiration:

[Aragon DAO](https://github.com/aragon/aragon-apps/blob/05d7692fcfb9cf5bc25a96674e09825defa2bbf3/apps/voting/contracts/Voting.sol) has a simple voting mechanism where actors can vote with their own tokens, power of their vote is linearly proportional with the actor's tokens. 
  - [`changeSupportRequiredPct(uint64 _supportRequiredPct)`](https://github.com/aragon/aragon-apps/blob/05d7692fcfb9cf5bc25a96674e09825defa2bbf3/apps/voting/contracts/Voting.sol#L95)
  - [`changeMinAcceptQuorumPct(uint64 _minAcceptQuorumPct)`](https://github.com/aragon/aragon-apps/blob/05d7692fcfb9cf5bc25a96674e09825defa2bbf3/apps/voting/contracts/Voting.sol#L110)
  - [`newVote(bytes _executionScript, string _metadata)`
- MakerDAO
  - 
- Aave DAO
  - newProposal
  - submitVoteByVoter
  - cancelVoteByVoter
  - tryToMoveToValidating
  - challengeVoters
  - resolveProposal
  - no meta voting (will be added in the future)

- [MolochDAO](https://github.com/MolochVentures/moloch)

Features we will implement for BarnBridge DAO:
- It can upgrade itself - a proposal can be created to self upgrade the DAO.
- The tokens are not locked in the contract - the tokens are still in the actor's balance, they are not transferred to the DAO.
- The proposal is an individual smart contract - the code which will be run, when the proposal is passed, is easily readable. The source code should be available on [Etherscan.io](https://etherscan.io) for anyone to read.
- Actors can cancel their vote - cancelling the vote will remove their voting power for that proposal.

## Specification

The DAO MUST implement the [EIP-2535 Diamond Standard](https://eips.ethereum.org/EIPS/eip-2535). This means that in the future more functionality can be added to the governance contract. An example of a functionality that is useful and will not change how the DAO works, but it will improve the usability, is the ability to vote using *meta transactions*. 

Configurable parameters:

- minimum voting threshold - a minimum absolute number (or percentage) that the proposal should have in order to pass or fail
- minimum proposal duration - a minimum time that a proposal needs to be open in order to collect the needed votes
- 



The technical specification should be detailed and should describe the syntax, semantics and interface of any feature. The specs should be detailed enough to allow technical discussions and ideation on top of this. 

## Implementation (optional)

The implementation can be added and updated before the **Frozen** state of the BBIP. It can be still be updated before that, even in the **Accepted** state.

This does not need to be the only implementation, but a canonical implementation is useful.

## Copyright

To be decided.