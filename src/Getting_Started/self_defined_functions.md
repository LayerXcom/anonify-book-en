
You can implement self defined logic on Anonify.

## Implement enclave memory

You can use `impl_memory` macro to implement enclave memory.
For example, [ERC20 tutorials](/Tutorials/ERC20/) implement 4 memories like followings.

```rust
impl_memory! {
    (0, "Balance", U64),
    (1, "Approved", Approved),
    (2, "TotalSupply", U64),
    (3, "Owner", UserAddress)
}
```

You specify the 3 arguments, first is the ID on enclave, second is the name of the memory and third is a type.

## 独自関数の定義

You can implement self-defined functions by `impl_runtime` macro.
The following is an exmple of implementation of `transfer` function using in [ERC20 tutorials](/Tutorials/ERC20/transfer/).


```rust
impl_runtime!{
    #[fn_id=1]
    pub fn transfer(
        self,
        sender: UserAddress,
        recipient: UserAddress,
        amount: U64
    ) {
        let sender_balance = self.get_map::<U64>(sender, "Balance")?;
        let recipient_balance = self.get_map::<U64>(recipient, "Balance")?;

        ensure!(sender_balance > amount, "transfer amount exceeds balance.");

        let sender_update = update!(sender, "Balance", sender_balance - amount);
        let recipient_update = update!(recipient, "Balance", recipient_balance + amount);

        insert![sender_update, recipient_update]
    }
}
```

`update` and `insert` macro enable us to run state transition.
