---
bbip: <to be assigned>
title: BOND token
torchbearer: Daniel Luca (@CleanUnicorn)
author: Daniel Luca (@CleanUnicorn)
discussions-to: <URL, usually the pull request>
status: Draft
created: 2020-09-02
---

## Simple summary

This describes the governance token which will be at the core of the whole BarnBridge system.

The synopsis of the improvement proposal. It needs to be short, clear and capture the essence of the proposal without going into any details. Provide an almost [ELI5](https://www.urbandictionary.com/define.php?term=ELI5) explanation.

## Abstract

It is a simple [ERC-20 token](https://eips.ethereum.org/EIPS/eip-20) to keep track of the $BOND token, which is a fungible asset. Any token is exactly equal to any other token, no special permissions will be available to the founding team, investors or advisors. Even more the founding team, investors and advisors will be on a vesting schedule of 1 year, meaning that they cannot use the full amount of tokens until the end of the vesting period. The vesting schedule will be described in a different BBIP (//TODO: add link when created).

All of the tokens will be generated when the contract is deployed, no new tokens will be minted later. The total number of tokens will be 10.000.000 (ten million). Those tokens will be split according to the whitepaper [distribution breakdown](https://github.com/BarnBridge/BarnBridge-Whitepaper#31-distribution). Because most of the systems do not exist at the moment, such as vesting contracts, DAO treasury, community pools, the tokens will be held by the [LaunchDAO](https://client.aragon.org/#/barnbridgelaunch/0x48fcf8dbc58fe970cbaa4c69c66fd58ec19cfbfd/). After each system is built, the LaunchDAO will send those tokens to the deployed contract, releasing control over more and more tokens, until all the tokens will be held by the proper system. This means that the trust in the system increases while the systems are built, eventually following a trustless system, where no party needs to trust the others in order to cooperate.

## Specification

One of the best ways to build ERC-20 contracts is to use the [OpenZeppelin contracts](https://github.com/OpenZeppelin/openzeppelin-contracts). They are industry standard at this point and have been battle tested.

Our contract will inherit the [ERC20](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol) contract and add a bit of functionality on top.

### Define basic token parameters

The token parameters are:

- `name` - BarnBridge Governance Token
- `symbol` - BOND
- `decimals` - 18

### Mint 10.000.000 tokens

Minting the tokens will be done ONLY on deploy. No other minting will be possible. 

### Deposit all said tokens in the LaunchDAO

Tokens should not be minted for the [Finance](https://client.aragon.org/#/barnbridgelaunch/0x3bc45731f72ecc4f41b864588d77f0852e2cf7e8/) address directly. The [Finance app](https://wiki.aragon.org/archive/dev/apps/finance/) needs to know how many tokens it holds. That's why sending tokens to the **Finance app** should NEVER happen.

The proper way to send tokens to the Finance app is described in [their documentation](https://wiki.aragon.org/archive/dev/apps/finance/). 

#### Approve ERC-20 tokens 

```solidity
token.approve(finance, amount)
```  

Parameters:
- `finance` - `0x3bc45731f72ecc4f41b864588d77f0852e2cf7e8`.
- `amount` - `10000000 * 10e18`.

#### Deposit tokens

```solidity
finance.deposit(token, amount, string reference)
```

Parameters:
- `token` - The address of the ERC-20 token. Because this is done in the contract itself, `address(this)` will represent the token address. The address can also be predicted as [described here](https://ethereum.stackexchange.com/a/761/6253).
- `amount` - `10000000 * 10e18` - this amount MUST match the value specified when minting tokens and approving tokens.
- `string reference` - //TODO: @DannyDesert can you provide a string? This will be visible along in the Finance transfers list.

### Owner

No owner needs to exist because no party has special permissions over the token.

### Burnable

Burn functionality can exist, users should be able to burn their own tokens.

### Pausable

Contract should not be pausable.

## Implementation (optional)

The implementation should heavily rely on the [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts/tree/master/contracts/token/ERC20) implementation.

In order to have the highest trust in the implementation, none of the original files should be modified. This means that the security audit should spend minimum time checking the OpenZeppelin implementation.

The provided implementation by OpenZeppelin will be inherited and extended with the custom logic described above.

## Copyright

[Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt)