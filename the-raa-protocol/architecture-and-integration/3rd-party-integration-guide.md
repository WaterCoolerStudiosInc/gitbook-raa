---
icon: plug
---

# 3rd Party Integration Guide

## Depositing and Redeeming sSTRK Tokens

Interacting with the Raa Staked Starknet (sSTRK) contract involves two primary actions: depositing native STRK to receive sSTRK shares and redeeming sSTRK shares to receive STRK back. This process incorporates a staking mechanism, a fee structure, and a batched unbonding process for redemptions.

### Depositing STRK

Users can deposit native STRK into the `StakedStarknet` contract to obtain sSTRK shares, which represent their proportional ownership of the total pooled STRK within the protocol. There is one available function for depositing: `deposit` and it is a one-step processes which will immediately exchange STRK for sSTRK tokens.

* **`deposit(address receiver, u128 assets, u128 min_shares)`**: When calling this function, the amount of STRK to deposit is specified via `assets` parameter. The `receiver` address will receive the newly minted sSTRK shares.

The function also triggers an internal `_update_fees()` call to ensure the protocol fees are accounted for before calculating the redemption ratio and minting shares. A `Deposit` event is emitted upon successful completion.

### Redeeming sSTRK

Redeeming sSTRK shares for native STRK is a two-step process designed to manage the unbonding of staked assets from the underlying protocol. This process involves requesting an unlock, a waiting period, and then finally redeeming the STRK.

#### **Requesting an Unlock (`request_unlock(address owner, uint128 shares, u128 min_assets)`)**:

1. Users initiate the redemption process by calling `request_unlock`, specifying the amount of sSTRK `shares` they wish to redeem.
2. The specified sSTRK shares are transferred from the user to the `StakedStarknet` contract.
3. The unlock request is added to the current batch and submits the batch if the batch wait time is already over.
4. The requested shares are added to the `batch_requests` mapping for the corresponding `batchId`.
5. Each user's unlock requests are tracked in the `user_unlock_requests` mapping, allowing a user to have multiple pending unlock requests (multiple requests from a user in a single batch is clubbed as one).

{% hint style="warning" %}
**Important:** Shares requested for unlock are put in escrow and cannot be transferred or used for further staking.
{% endhint %}

#### **Sending Batch Unlock Requests (`submit_batch()`)**:

1. This function can be called by anyone, but is intended to be automated to process completed unlock request batches.
2. It submits the current batch for unbonding and it updates the `batch_id` number.
3. The total aggregated shares from the processed batches are burned from the contract's total supply.
4. It initiates the unbonding process with the underlying nodes, reducing the `totalPooled` amount.

{% hint style="info" %}
**Note:** This step continues the unbonding process but does not immediately return STRK to the users.
{% endhint %}

#### **Redeeming Unlocked STRK (`redeem_unlock_request(felt252 batch_id, address owner, address receiver)`)**:

1. Users can call this function to claim their unlocked STRK after the `COOLDOWN_PERIOD` has passed since the `submissionTime` of the batch containing their unlock request.
2. The user specifies the `batch_id` corresponding to their specific unlock request within their `userUnlockRequests` array.\
   The function checks that the specified `batch_id` is valid and that it has been submitted (`batch_info.state == BatchState::Executed`) and the unpool\_time period has elapsed (`block.timestamp > batch.state.unpool_time`).
3. The user's specific unlock request is deleted from their `userUnlockRequests` array.
4. The calculated `assets` (STRK) are transferred to the specified `receiver` address.

#### Cancelling Unlock Requests (`cancel_unlock_request(felt252 batch_id)`)

Users have a limited window to cancel an unlock request.

* A request can only be cancelled within the _same batch interval_ in which it was originally sent.
* The user specifies the `batch_id` of the request they wish to cancel.
* The function verifies that the request exists and that the current batch ID matches the batch ID of the request.
* The shares from the cancelled request are removed from the corresponding `batch_unlock_requests`.
* The unlock request is deleted from the user's `user_unlock_requests` array.
* The sSTRK shares are transferred back to the user.

## Key Considerations for Users

* **Redemption is Two-Step:** Unlike some staking mechanisms, redemption is not instant. It requires requesting an unlock and then waiting for a cooldown period after the batch is processed.
* **Batching:** Unlock requests are processed in batches. The timing of when a batch is sent for unbonding depends on external calls to the `submit_batch` function.
* **Cooldown Period:** After a batch is sent for unbonding, there is a `COOLDOWN_PERIOD` before the redeemed STRK can be claimed.
* **Cancellation Window:** Unlock requests can only be cancelled within the same batch interval they were initiated. The active batch can be found via the `get_current_batch_id` function.

{% hint style="success" %}
Make sure to check out the `StakedStarknet` [contract interface](stakedstarknet-contract.md#stakedstarknet-contract-interface) for more details on how to interact with it.
{% endhint %}
