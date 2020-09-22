---
bbip: 2
title: Yield Farming
torchbearer: Troy Murray (@DannyDesert)
author: Troy Murray (@DannyDesert)
discussions-to: https://github.com/BarnBridge/BBIP/pull/4
status: Preview
created: 2020-08-28
---

## Simple summary

A simple way for future users of the protocol to get access to the $BOND token before the Smart Yield Bond product launches.  

## Abstract

Participants can harvest their yield at the conclusion of each epoch. Each epoch will last 1 week, and an equal number of $BOND tokens will be distributed during each epoch. The participants’ harvest will be based on the amount of stable coins they have staked relative to the total amount staked in the pool. The calculation will be time-weighted to promote true and fair pro rata harvesting. Any participant can add to the pool during the duration of an epoch and receive rewards proportional to the time they are staked, but the funds must stay staked through the end of the epoch to be able to harvest the rewards. That’s Week 1 - the first epoch - plain and simple.  

Starting Week 2 (the second epoch), participants will have the opportunity to turbocharge their $BOND farm and juice their crops with a powerful rewards multiplier. By claiming your $BOND harvest and adding it to the Uniswap v2 liquidity pool in exchange for BOND/USDC LP Token, then staking this LP token back in the BarnBride Yield Farming contract, you will begin earning 2.5x $BOND distribution. We would like to emphasize that a contract like this has never been written before; one that allows users to stake different stable tokens in a single contract while assigning a rewards multiplier for those who stake the LP token. We are proud to give this to the open source/WEB3 community for use in future projects if any find it fitting.

## Specification

- Total $BOND Token Supply: 10,000,000
- Token amount supplied to Yield Farming: 800,000 or 8%
- Tokens available to stake: USDC, DAI, & sUSD, USDC_BOND_UNI_LP (2.5x)
- Yield Farming Duration: 24 weeks
- Epoch length: 1 week
- Tokens distributed in each epoch: 33,333.3333333333


Current breakdown: https://docs.google.com/spreadsheets/d/1AKdLHFsIujM-MS3atlrKUsLMXelIHhWQsHOQh2iFEIs/edit?usp=sharing

## Implementation (optional)



## Copyright

[Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt)
