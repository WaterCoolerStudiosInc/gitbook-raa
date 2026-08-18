---
description: >-
  This is the core contract for the protocol, and implements the Vault (a.k.a.
  the Raa stake pool).
icon: code
---

# StakedStarknet Contract

## Overview

The `StakedStarknet` contract - also known as the "vault" or the "Raa stake pool" - sits at the heart of the Raa Liquid Staking process. This contract is the user-facing entry point to the Raa Protocol, and serves a number of functions:

1. **Core functionality:** The `StakedStarknet` contract is the core contract of the sSTRK token. It is an ERC-20 token contract with extended functionality.
2. **User entry point:** The `StakedStarknet` contract is the user-facing interface to the protocol. Users who want to participate in staking yield can deposit STRK to the `StakedStarknet` contract and receive sSTRK tokens in return. Users can also request to redeem their sSTRK for staked STRK along with their pro-rata share of the yield accrued by the protocol.
3. **Orchestrates staking delegation & redemption:** The `StakedStarknet` contract delegates staking tokens to, and interfaces with, [Validators](../../overview/proof-of-stake-blockchains.md#staking-and-validators), according to the [Target Weights](stakedstarknet-contract.md#target-weights) . This is done using Raa's [Constant Retargeting Algorithm](staking-and-un-staking-mechanisms.md#constant-regargeting-algorithm).
4. **Calculates Protocol Fees:** The `StakedStarknet` contract calculates and stores [Virtual Shares](../definitions.md) used to facilitate protocol Management Fees. For more information, see the docs on Governance & Management Fees.
5. **Inherits the Registry Interface:** The `Registry` interface maintains a list of [Validators](../../overview/proof-of-stake-blockchains.md#staking-and-validators) that are actively participating in the Raa protocol, along with a set of Target Weights for allocation of staked STRK to those Validators.

{% hint style="info" %}
Note: the Redemption Ratio is not stored as a variable, however the `StakedStarknet` contract contains methods to calculate it in both directions (from sSTRK to STRK, and vice versa).
{% endhint %}

### Registry Interface Variables

#### Target Weights

These weights represent the target percentage of the total amount of staked STRK allocated to each Validator. These target weights are meant to uphold decentralization of the protocol, and will be controlled by Governance in a decentralized way.
