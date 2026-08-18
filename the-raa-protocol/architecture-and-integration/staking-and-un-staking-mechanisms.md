---
description: What happens behind the scenes when a user stakes or unstakes with Raa
icon: gear-code
---

# Staking and Un-staking Mechanisms

## **Staking Mechanism**

Staking on Raa involves the following order of operations:

1. **STRK Deposit:** User deposits STRK into the [`StakedStarknet`](stakedstarknet-contract.md#the-vault-contract) contract by initiating a `stake` transaction.
2. **Validator Forwarding:** During stake, `delegateBondingto` is called, retrieving the current [Target Weights](stakedstarknet-contract.md#target-weights). It then checks all participating Validators to see how many STRK are currently allocated to each. These sums are then used to calculate the Current Weights (the percentage of the Total Pool held by each Validator). Finally the [Constant Retargeting Algorithm](staking-and-un-staking-mechanisms.md#constant-regargeting-algorithm) is triggered to calculate how many of the newly submitted STRK to forward to each of the Validators, and forwards them accordingly.
3.  **Receipt Token Issuance:** The `StakedStarknet` contract calculates the correct number of new shares (sSTRK) to create, using the current [Redemption Ratio](../definitions.md), and transfers them to the user. These sSTRK serve as a receipt entitling the holder to their share of the [Total Pooled](../definitions.md). The sSTRK tokens are fungible, can be traded and used elsewhere on the network, and can be used to redeem STRK during the unstaking process. The number of new sSTRK created, denoted here as _newShares,_ is as follows:<br>

    $$newShares = newStake * (totalShares/totalPooled)$$<br>

    where _newStake_ is the number of newly deposited STRK, _totalShares_ is the [Total Shares](../definitions.md) and _totalPooled_ is the [Total Pooled](../definitions.md).

To see how this translates into a user's experience staking on Raa, see our docs on [Staking with Raa](../staking-and-unstaking-with-raa.md#staking-with-raa).

### Constant Retargeting Algorithm

As the [Vault](stakedstarknet-contract.md#staked-monad-contract-functions) accepts new STRK for staking it must delegate them across the participating Validators in such a way that aims to keep the amount delegated to each Validator consistent with the [Target Weights](stakedstarknet-contract.md#target-weights) stored in the [Registry](stakedstarknet-contract.md#registry-interface-variables). In order to to do this, the Vault retrieves the Target Weights from the Registry and compares them to the current percentages of the Total Pool held by each of the participating Validators at that time. The Vault then calculates how much of the newly added STRK should be delegated to each Validator in order to bring the current weights closer to the Target Weights stored in the Registry.

During the unstaking process, the Vault again uses the Target Weights from the Registry to decide how many STRK to redeem from each Validator.

***

## Unstaking Mechanism

Raa involves some clever mechanisms to enable unstaking at scale, despite network-level constraints such as the Cooldown Period and [Daily Unbonding Request Limits](staking-and-un-staking-mechanisms.md#daily-unbonding-request-limits).

### Unstaking Order of Operations

Unstaking on Raa involves the following order of operations:

1. **Initiation:** User calls [Request Unlock](contract-interface-abi-and-functions.md#unstaking-functions), during which they submit their sSTRK to the Vault.
2. **Request added to Current Batch**: The uses unlock request is added to the current batch of unlocks, to be submitted to validators.
3. **Batch Unlock Requests sent:** After the batch window closes for the current batch, the unlock requests are sent to validators. These requests leverage the [Constant Retargeting Algorithm](staking-and-un-staking-mechanisms.md#constant-retargeting-algorithm) to keep withdraws as evenly as possible.
4. **Cooldown Period:** User must wait until [Cooldown Period](staking-and-un-staking-mechanisms.md#cooldown-period) is over so that Validators are able to unbond the STRK tokens.
5. **Tokens returned to Vault:** Once the cooldown period has ended, the first user who calls `redeem`, also triggers the `fulfill_batch` method on the `StakedStarknet` contract (a.k.a. the Vault). This forwards all newly unbonded STRK held by the Validators back to the Vault.
6. **Token redemption:** This is where the user can redeem their STRK, including accrued yield. In order for this to happen, the Vault must have custody of the unbonded STRK. There are two scenarios here:
   1. If the newly unbonded STRK are still held by Validators when a user tries to redeem theirs, the `fulfill_batch` method is called under the hood, during which the `Vault` collects the unbonded STRK from each of the Validator, and then executes the [`redeem`](contract-interface-abi-and-functions.md) call, which forwards the user's entitled portion of those STRK from the `Vault` to the user's wallet.
   2. If the Vault already has custody of the STRK, the user can make a [`redeem`](contract-interface-abi-and-functions.md) call to the `Vault`, which sends the user's newly unstaked STRK to their wallet.

To see how this translates into a user's experience staking on Raa, see our docs on [Unstaking with Raa](../staking-and-unstaking-with-raa.md#unstaking-with-raa).

### Request Routing
