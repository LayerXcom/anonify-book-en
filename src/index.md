# Anonify

Anonify is a privacy-protected system that enables the blockchain to be used as a tamper-proof data-sharing infrastructure using a Trusted Execution Environment (TEE) to enable high availability and flexible execution of business logic while protecting data that is not wanted to be revealed between participants in a distributed ledger. In addition, it provides an audit function that can read the data in the distributed ledger for certain entities.

<div align="center">
<img src="https://user-images.githubusercontent.com/20852667/81777586-203a1b00-952c-11ea-9757-705fd2ca6995.png" width="800px">
</div>

Instead of recording the history of state transitions in plain text like a normal blockchain, Anonify records the state data encrypted with TEE in the blockchain, calculates the state transitions in the isolated protected area of TEE, encrypts the results and records them in the blockchain. That is, the blockchain records the history of all state transitions as ciphertext, while TEE records the latest state as plaintext. The data in the encrypted area is protected from the outside at the hardware level, so that neither the administrator who runs the TEE nor the system software such as the OS can access it.

## References

- White Paper
- [Github](https://github.com/LayerXcom/anonify)
