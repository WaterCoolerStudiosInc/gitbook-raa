---
description: Terminology for the Raa Protocol
icon: spell-check
---

# Definitions

* **Gas Token:** The native token for a given blockchain. For example, the Gas Token on Starknet is STRK. For more info, please review the page on [Proof-of-Stake Blockchains](../overview/proof-of-stake-blockchains.md).
* **sToken**: A general form for Liquid Staking Tokens. sTokens are fungible, can be traded and used elsewhere on the network, and can be used to redeem Gas Tokens during the unstaking process.
* **sSTRK**: Raa's Liquid Staking Tokens on Starknet. sSTRK serve as a receipt entitling the holder to their share of the `Total Pool` in the `Vault`. sSTRK tokens are fungible, can be traded and used elsewhere on the network, and can be used to redeem STRK during the unstaking process.
* **Validator**: an entity on the blockchain that stakes Gas Tokens (STRK), validates transactions on behalf of the network, and is rewarded with yield on their Gas Tokens in return for the service. For more information, see [Proof-of-Stake Blockchains](../overview/proof-of-stake-blockchains.md#staking-and-validators).
* **DEX:** Abbreviation for a [Decentralized Exhange.](https://en.wikipedia.org/wiki/Decentralized_finance#Decentralized_exchanges)
* **Bonding / Unbonding:** A process by which tokens can be "frozen" in exchange for some other benefit. This is commonly used to refer to the process by which the blockchain locks tokens up, or unlocks them, during the staking process. It can sometimes be used interchangeably with the terms "staking" and "unstaking".
* **Delegation (aka Nomination):** Also sometimes referred to as "nomination", this is the process by which a user "delegates" another entity to do something on their behalf. In the case of Liquid Staking, users can "delegate" a "Delegation/Nomination Pool" to stake their tokens on their behalf with a Validator. Note that Raa does not use Nomination Pools, but rather nominates directly yo Validators.
* **Total Pooled:** The sum of all Gas Tokens currently deposited in a Raa [`Vault`](architecture-and-integration/stakedstarknet-contract.md#the-vault-contract), plus all yield that has been compounded during network staking.
* **Total Shares:** Denotes the total outstanding supply of sTokens plus the number of Virtual Shares eligible to be claimed by the DAO.
  * **Total Virtual Shares:** This represents the revenue entitled to the DAO. It denotes the number of sTokens that are eligible to be minted during the **DAO Revenue Redemption Process**. These are tracked on the `Vault`.
  * **Total Shares Minted:** The total outstanding supply of sTokens on a given network. sTokens are minted during the [staking process](architecture-and-integration/staking-and-un-staking-mechanisms.md#staking-mechanism) and are burned during the [unstaking process](architecture-and-integration/staking-and-un-staking-mechanisms.md#unstaking-mechanism).
* **Redemption Ratio:** This is the ratio of `totalPooled` / `totalShares`. This ratio can be calculated at any time and is used to inform how many Gas Tokens can be redeemed by a user for each sTokens token they submit during the [un-staking ](architecture-and-integration/staking-and-un-staking-mechanisms.md#unstaking)process. This ratio can also be used by market makers to keep exchange rates honest on [DEXes](definitions.md). Note: the Redemption Ratio is not stored as a variable, however the `Vault` contract contains methods to calculate it in both directions (e.g. from sSTRK to STRK, and vice versa).
* **Era:** A blockchain-native construct denoting the period that the Validator set (and each Validator's active delegator or nominator set) is recalculated and where staking rewards are paid out to Validators.
* **Batch**: A lump sum of unlock requests that are sent to to validators according to current stake imbalances.
