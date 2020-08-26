---
bbip: 1
title: BarnBridge DAO
torchbearer: Daniel Luca (@CleanUnicorn)
author: Daniel Luca (@CleanUnicorn)
discussions-to: https://github.com/BarnBridge/BBIP/pull/3
status: Preview
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

- [Aragon DAO](https://github.com/aragon/aragon-apps/blob/05d7692fcfb9cf5bc25a96674e09825defa2bbf3/apps/voting/contracts/Voting.sol)
- [MolochDAO / MetaCartel](https://github.com/MolochVentures/moloch)
- [Compound DAO](https://github.com/compound-finance/compound-protocol/blob/master/contracts/Governance/GovernorAlpha.sol)
- [Livepeer](https://github.com/livepeer/protocol/blob/1074b62435a6272470d40dc8ab3e3d80d5c2b634/contracts/polling/PollCreator.sol)
- [Aave DAO]() TODO: add link - code not public yet (Ernesto said code will be public next week)

Features we will implement for BarnBridge DAO:
- It can upgrade itself - A proposal can be created to self upgrade the DAO.
- The tokens are not locked in the contract - The tokens are still in the actor's balance, they are not transferred to the DAO.
- The proposal is an individual smart contract - The code which will be run, when the proposal is passed, is easily readable. The source code should be available on [Etherscan.io](https://etherscan.io) for anyone to read.
- Actors can cancel their vote - Cancelling the vote will remove their voting power for that proposal.
- Actors can change their vote - This is a combination of vote cancelling and a normal vote.

Features we might implement in the future:
- Deposit when proposal is created - Is a deposit needed when the proposal is created? Will this deposit be returned or lost in any special case?
- Anyone can create proposals - We need to discuss whether anyone can create proposals or only some actors. This needs to be clear right from the start; maybe only $BOND token owners can create proposal, or only the founders / investors / advisors.
- Do we want to punish people if they don't vote?
- Do we want to reward people if they vote?
- Meta transactions
- Vote delegation

## Specification

The DAO MUST implement the [EIP-2535 Diamond Standard](https://eips.ethereum.org/EIPS/eip-2535). This means that in the future more functionality can be added to the governance contract. 

An example of a functionality that can be added after the initial launch, which will improve the usability, is the ability to vote using *meta transactions*. Using meta transactions will allow users to store their tokens in a cold storage, while still being able to vote with those tokens. Another similar approach is to delegate votes to another address (hot wallet).

Configurable parameters (not constants):

- Minimum voting threshold - a minimum absolute number (or percentage) of votes that the proposal should have in order to pass or fail
- Minimum proposal duration - a minimum time that a proposal needs to be open in order to collect the needed votes
- Minimum grace period (optional / if needed) - a time interval where the votes can be contested by other parties. In case we delegate votes or do meta transactions, this period needs to exist to make sure no double voting happens.

### Key methods (WIP)

- **propose** - creates a new proposal
  - **executor** - the address of the contract that holds the code which will be executed when the proposal passes
  - **votingBlocksDuration** - the number of blocks this proposal remains open, while waiting for the minimum number of votes
  - **challengeBlocksDuration** - the number of blocks this proposal stays in challenge state, where actors can reveal double voting

- **vote** - cast a vote on an existing proposal. It can also be used to replace an existing vote.
  - **proposalID** - unique identifier of the proposal
  - **approve** - true / false

- **cancelVote** - remove one's vote
  - **proposalID** - unique identifier of the proposal

- **challengeVote** - use this in case double voting happened
  - **proposalID** - unique identifier of the proposal
  - **voters[]** - list of voters that do not hold the tokens they voted with

- **executeProposal** - after the proposal has gone through all necessary steps, execute it
  - **proposalID** - unique identifier of the proposal

### Proposal

The proposal should be a separate contract that holds the related details. This means that the `executor` contract must implement this minimum interface:

```solidity
interface IProposal {
    // Returns the name of the proposal. This cannot be changed in the future.
    function name() external pure returns (string memory);

    // Returns a more detailed description of the proposal. This cannot be changed in the future.
    function description() external pure returns (string memory);
    
    // This is executed when the proposal is passed.
    function execute() external;
}
```

This interface should be implemented by the contract which defines the proposal. Here is an example:

```solidity
contract ExampleProposal is IProposal {
    event ExampleProposalExecuted();
    
    function name() public pure override(Proposal) returns(string memory) {
        return 'The proposal name';
    }
    
    function description() public pure override(Proposal) returns(string memory) {
        return 'A more detailed description of the proposal';
    }
    
    function execute() public override(Proposal) {
        emit ExampleProposalExecuted();
    }
}
```

This leaves the opportunity to expand the proposals, by just updating the frontend which pulls the information from the `executor` contract.

### States

- **Voting** - after a proposal was created it starts in the **Voting** state, where any token holder can vote
- **Validating** - after the required number of votes was accrued, the **Validating** stage allows any party to report double voting by anyone by calling `challengeVote`
- **Passed** - achieved after the **Validating** stage was successful, meaning it did not invalidate enough votes to push the proposal back to the **Voting** stage
- **Failed** - the proposal was rejected by the token owners
- **Expired** - the proposal did not gather enough votes before the expiration time 
- **Executed** - a **Passed** proposal can be executed by anyone

#### Flow diagram

Flow diagram below. Created and updated with [asciiflow](http://asciiflow.com/).

```
+----------+   Enough opposing votes  +-------------------------+  Not enough votes                +---------+
|          |   to cancel the proposal |                         |  within the allotted time        |         |
|  Failed  | <------------------------+         Voting          +--------------------------------> | Expired |
|          |                          |                         |                                  |         |
+----------+                          +-+-------------------+---+                                  +---------+
                                        |                   ^
                                        | Enough            | Enough
                                        | supporting        | votes are
                                        | votes             | invalidated
                                        | in the            | to go back
                                        | allotted          | to Voting
                                        | time              |
                                        |                   |
                                        |                   |
                                        v                   |
                                     +--+-------------------+---+
                                     |                          |
                                     |        Validating        |
                                     |                          |
                                     +--------------+-----------+
                                                    |
                                                    |
                                                    | Proposal
                                                    | still has
                                                    | enough supporting
                                                    | votes at the end
                                                    | of the Validating stage
                                                    |
                                                    |
                                              +-----+----+
                                              |          |
                                              |  Passed  |
                                              |          |
                                              +-----+----+
                                                    |
                                                    |
                                                    | Proposal
                                                    | is executed,
                                                    | code is run
                                                    |
                                                    |
                                                    v
                                            +-------+----+
                                            |            |
                                            |  Executed  |
                                            |            |
                                            +------------+
```

## Implementation (optional)

The implementation can be added and updated before the **Frozen** state of the BBIP. It can be still be updated before that, even in the **Accepted** state.

This does not need to be the only implementation, but a canonical implementation is useful.

## Copyright

[Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt)