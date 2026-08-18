---
description: Protocol Architecture & Functionality
icon: info
---

# How It Works

## Mission

Raa is a [Liquid Staking Protocol](../overview/liquid-staking.md). Our mission is to boost the GDP of Proof-of-Stake blockchains by allowing users to participate in on-chain activities while also benefitting from the yield-bearing staking that secures the blockchain. We do this by providing stakers with a liquid token which is backed by the blockchain's staked native gas tokens. We call this a Liquid Staking Token, abbreviated as `LST` .

{% hint style="info" %}
**Note:** Raa refers to its LSTs as "sTokens". For example, the Gas Token of Starknet is STRK, and the LST of Raa on Starknet is sSTRK.
{% endhint %}

## How it works

Raa improves on [Delegated Proof-of-Stake](../overview/proof-of-stake-blockchains.md#delegated-proof-of-stake-dpos) by allowing users to participate without themselves having to find and choose a validator to which to delegate. To accomplish this, Raa pools users’ Gas Tokens together and delegates them across a set of participating network [Validators](../overview/proof-of-stake-blockchains.md#staking-and-validators) for staking, and gives those users sTokens (the collateral) that can be used to redeem their staked Gas Tokens at any time. The protocol maintains a decentralized list of participating validators, routes staking requests to them, and facilitates redemption requests on behalf of the user.

The Raa Liquid Staking protocol automates the process by which users can pool their STRK tokens together for network staking via non-custodial smart contracts. By leveraging a decentralized Registry of Validators controlled by the DAO, Raa creates sSTRK, a liquid version of the core STRK gas token. As a ERC-20 token, sSTRK can be used in DeFi and other on-chain uses cases, while the underlying STRK is natively staked to earn rewards and secure the Starknet network while simultaneously empowering the community to Stake & Use.

In addition to being liquid, unlike native staking, the super power of Raa and other Liquid Staking protocols is in their composability. This means that developers of novel on-chain use cases and the community as a whole can hold sSTRK to stake, or collateralize it in other protocols so you don't need to forgo staking rewards while you DeFi.

Further, collectively the Raa protocol surpasses the minimum required amount of tokens to participate in staking, so there is no minimum stake to participate. No more "you must be this tall to ride." At the same time, those users can participate in other network activities such as DeFi, gaming, and more.
