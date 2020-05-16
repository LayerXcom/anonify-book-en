
Anonify is a privacy-protected system that allows the blockchain to be used as a tamper-proof data sharing infrastructure. Instead of recording state transitions in plain text like a normal blockchain, Anonify records the state data encrypted by TEE in the blockchain. Anonify calculates the state transitions in an isolated protected area of the TEE server, encrypts the results and records them in the blockchain. The data in the Enclave is protected as the hardware level from the outside, so that neither the TEE administrators themselves nor the system software, such as the operating system, can access it.

TEEs participating in Anonify network share the group key used for encryption and decryption via the blockchain. This allows a TEE to retrieve the ciphertext broadcasted to the blockchain and the other TEEs decrypt it in Enclave to update the latest state. In other words, the blockchain records the history of all state transitions as a ciphertext, while TEE records the latest state as a plaintext.

The state transition rules handled by Enclave are enforced by the Remote Attestation mechanism of TEE.TEEs participating in the network send the Remote Attestation signed result data (REPORT) to the blockchain and verify that it hasn't been tampered with by smart contract signature validation of the REPORT. Each TEE verifies that the hash value (MRENCLAVE) of the state transition rule executed in the Enclave included in REPORT matches the one registered at setup time. As long as MMRENCLAVE matches, all TEEs are guaranteed to execute the same state transition rule.


In other words, each TEE handles the state transition itself individually, and all the participants have to validate the state transition rules in a smart contract on the blockchain. This allows us to verify the correctness of the state transition rules executed in TEE between all participants, and to record the entire history of the resulting state transitions on the blockchain in an encrypted state.


<div align="center">
<img src="https://user-images.githubusercontent.com/20852667/81645749-16011980-9465-11ea-80c2-9fe3b11e0ff1.png" width="1400px">
</div>


The above is a diagram of the state transition in TEE and the workflow for broadcasting ciphertext to the blockchain. The State Cache can only be accessed by a client that holds specific authentication information through the Authentication layer. The State Cache keeps track of the latest state. Based on the state data and input data, it processes state transitions, encrypts them with a group key at the TreeKEM layer, and broadcasts them to the blockchain. The State Hash layer performs a hash calculation to verify state matching. The ciphertext recorded in the blockchain is read by TEE, decrypted again by TreeKEM, and then recorded in the State Cache as updated state data. Sealing provides the ability to encrypt the State Cache with an on-chip key and record it to external storage. Attestation is a feature that allows you to verify the integrity of your program when you register your TEE with the blockchain.