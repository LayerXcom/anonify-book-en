We explain the workflow of the registration phase where TEE participates in the Anonify network, the state transition phase where transactions are sent from outside to Anonify, and the key rotation phase where the group keys used for encryption are rotated. Here, we assume that the entity sending the transaction from outside has authentication information, such as a key that can access the Enclave.


## Registration Phase

This is the phase where the public key is registered in the blockchain after verifying that TEE is executing a legitimate state transition program. On-chain validation of REPORT obtained by Remote Attestation and registration of the Enclave Identity public key included in it to the on-chain. REPORT verification includes REPORTS signature verification and MRENCLAVE match verification. The registration phase is the phase that takes place when a specific state transition logic is determined and a new Anonify network is generated, when a TEE joins an existing Anonify network, or when the REPORT timestamp expires.


The workflow from the time TEE generates an Enclave to joining a specific group on Anonify is followings:

* Generate a closed Identity key pair for Enclave at the time of Enclave generation.
* Perform Remote Attestation. Include the Identity public key and Nonce in QUOTE, send it to the Attestation Service and receive the response of REPORT with a signature.
* Generate a handshake to share a group key
* Generate a transaction containing REPORT and handshake and broadcast it to the blockchain
* Through a series of REPORT verification on the blockchain, the registration of TEE is completed by recording the Identity public key on the blockchain. In the verification of the REPORT, if a new Anonify network is created, the MRENCLAVE is recorded in the blockchain. On the other hand, in case of joining the existing Anonify network, check whether the MRENCLAVE already recorded in the blockchain matches the MRENCLAVE presented.

## State transition phase

This is the phase where transactions are broadcast on the blockchain network to share the results of the cryptographic state transitions. A workflow where an external entity with credentials sends a transaction on Anonify, which is shared by the blockchain and reflected in the state of other TEEs is as follows: 

* An external entity sends the authentication information and state transition inputs required for access to the TEE
* The authentication information is validated in Enclave, and the data is input to a defined state transition function and calculated
* TEE encrypts the result, calculates the lock parameters, signs it with the Enclave Identity private key, and broadcasts the transaction with these data to the blockchain nodes
* A smart contract on the blockchain that records the ciphertext after the signature verification with the Enclave Identity public key
* All TEEs in the network obtain the ciphertext recorded in the blockchain and decrypt it in Enclave to update the state



## Key Rotation Phase

This phase rotates the group encryption keys shared through all TEEs. The workflow are follows: 

* TEE's operator client sends a request to TEE for a key rotation
* Generate a new key internally or get a key externally, depending on the request
* Calculate TreeKEM's group state update using the new key
* To share the new state of TreeKEM to other TEEs, encrypt it using the public keys of the TEE members and broadcast it to the blockchain
* Each TEE decrypts the fetched ciphertext with its own private key, updates the group state of TreeKEM, and obtains a common group seed value for each.
