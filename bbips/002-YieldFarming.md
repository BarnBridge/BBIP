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

In the true spirit of decentralization, we need a way to distribute $BOND tokens to future user of the protocol that couldn't get access to the Seed round.  There will be a staking pool that uses the assets (DAI, USDC, & sUSD) that will be used in the first product of the protocol which is the Smart Yield Bond.  8% of the initial token supply (800,000) will be allocated to the staking pool.

## Specification

- Each epoch is 1 week long
- Number of distributed tokens is equal for all epochs; each epoch emits = 800.000 / number of epochs = 33,333.33 tokens
- It's the fairest way to distribute because the people who did not join in the beginning, can still join later and not be severely diluted
- The dilution will be controlled by the popularity of the $BOND tokens (this is the same for Bitcoin, you earn less if more people join; but it's valued more)
- It simplifies the model and makes it easier to understand

Current breakdown: https://docs.google.com/spreadsheets/d/1AKdLHFsIujM-MS3atlrKUsLMXelIHhWQsHOQh2iFEIs/edit?usp=sharing

## Implementation (optional)



## Copyright

[Apache License Version 2.0](https://www.apache.org/licenses/LICENSE-2.0.txt)
