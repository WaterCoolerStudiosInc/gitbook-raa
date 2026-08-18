---
description: Details on select functions, including previous versions when relevant.
icon: function
---

# Contract Interface, ABI, & Functions

## Contract Addresses

{% content-ref url="../official-contract-addresses.md" %}
[official-contract-addresses.md](../official-contract-addresses.md)
{% endcontent-ref %}

{% hint style="warning" %}
Full ABIs can be found in our published npmjs package. These condensed interfaces contain the most important functions for integrating with the Raa core protocol.
{% endhint %}

## StakedStarknet Condensed Interface

```cairo
pub type Amount = u128;
pub type Weight = u128;
pub type Seconds = u64;

#[starknet::interface]
pub trait IStakedStarknet<TState> {
    // Setters
    fn deposit(ref self: TState, receiver: ContractAddress, assets: Amount, min_shares: Amount) -> Amount;
    fn donate_to_vault(ref self: TState, beneficiary: ContractAddress, amount: Amount);
    fn request_unlock(ref self: TState, owner: ContractAddress, shares: Amount, min_assets: Amount) -> BatchId;
    fn cancel_unlock_request(ref self: TState, batch_id: BatchId);
    fn redeem_unlock_request(ref self: TState,batch_id: BatchId, owner: ContractAddress, receiver: ContractAddress) -> Amount;
    fn submit_batch(ref self: TState) -> Seconds;
    fn fulfill_batch(ref self: TState, batch_id: BatchId);
    fn sync_staking(ref self: TState);
    fn compound(ref self: TState, agent_list: Option<Array<ContractAddress>>);

    // Getters
    fn get_batch_state(self: @TState, batch_id: BatchId) -> BatchState;
    fn get_batch_record(self: @TState, batch_id: BatchId, user: ContractAddress) -> Record;
    fn get_user_unlock_requests(self: @TState, user: ContractAddress) -> Array<BatchId>;
    fn convert_to_shares(self: @TState, assets: Amount) -> Amount;
    fn convert_to_assets(self: @TState, shares: Amount) -> Amount;
    fn get_pool_weight(self: @TContractState, pool_address: ContractAddress) -> Option<Weight>;
    fn get_all_pool_addresses(self: @TContractState) -> Array<ContractAddress>;
    fn get_virtual_shares(self: @TContractState) -> Amount;
    fn get_all_agents(self: @TState) -> Array<ContractAddress>;
}
```

## StakedStarknet contract ABI

{% file src="../../.gitbook/assets/StakedStarknet.json" %}
