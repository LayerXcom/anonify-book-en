We explain the key technical elements that make up Anonify.

## Blockchain

Blockchain technology enables nodes participating in the network to verify the correctness of state transitions with each other and record and share cryptographically unfalsifiable state data. Anonify uses blockchain as a platform for processing the verification of the integrity of state transitions performed in TEE, and for sharing the history of encrypted state transitions among multiple entities.
The history of encrypted state transitions is replicated between nodes, so that a TEE that holds a decryption key can always access the latest state which is agreed.

## TEE (Trusted Execution Environment)

TEE (Trusted Execution Environment) is an extension of the processor that ensures that certain applications are run in an Enclave that is isolated from other software. The basic security requirements and interface definitions for TEE are provided by the standards institution [GlobalPlatform](https://globalplatform.org/).  

TEE uses a hardware-level design to ensure that the kernel OS, hypervisor, and other authoritative system software cannot gain unauthorized access to the memory in the protected area. This means that your application is also protected against malware from system software. These security features allow general-purpose programs to be executed securely in a much more isolated environment than in the past, which has led to a growing number of applications that involve highly sensitive data.  

Typical example of TEE are Intel SGX, ARM TrustZone and RISC-V Keystone, and also AWS Nitro Enclaves are of a similar property. In addition to memory isolation protection, there is also a TEE with a feature called Attestation, which ensures that the intended executable binary is running on a legitimate processor. In summary, TEE has the property of providing the integrity and confidentiality of the executable binary.

Also, TPM (Trusted Platform Module) is a typical standard for processor-level isolated execution; TPM differs greatly in that programs are hard-coded into the chipset, while TEE opens up the implementation of isolated execution programs to general developers. Google Titan, Apple T2, and Microsoft Pluton also have similar features, but implementations and updates are limited to product vendors only.

### Intel SGX

Intel SGX (Software Guard Execution) is a security feature in processors on the sixth generation Intel Skylake microarchitecture and later. At processor startup, the SGX allocates a fixed size protection area as a subset of DRAM and allocates virtual memory as an enclave area. The protected area is controlled by the processor and cannot be accessed by the system software or even via DMA.

In addition, the memory in the Enclave is encrypted and recorded for side-channel attack resistance, and then decrypted again during processing by the processor. In addition, the software in Enclave can only execute instructions with Ring3 (user-land) privileges in Ring protection, and cannot run applications with strong privileges such as Ring0. EENTER and EEXIT, which are specific instructions for accessing inside and outside of Enclave, are also Ring3 instructions.

SGX features sealing and attestation, which can derive on-chip keys and program-dependent keys in Enclave, and encrypt them in Enclave. The ciphertext can then be exported externally and stored on a disk. If you want to restore it, you can import the ciphertext into Enclave in the same way and decrypt it by Unsealing with the same key. Another functional property, Attestation, will be discussed below.


### Attestation

Intel SGX confidentiality is provided by Enclave's isolation protection, but this alone does not allow a third party to verify the integrity of the processor or the programs running within Enclave.
The Remote Attestation feature provides an integrity. In other words, the Remote Attestation Service ensures that the correct executable is working within a particular Enclave.
In contrast, Local Attestation is a protocol feature that allows Enclaves on the same machine to verify with each other that they are indeed running on the same machine, but here we discuss a particularly important Remote Attestation feature in Anonify.  

Remote Attestation can send a data structure called QUOTE containing the security status of a particular SGX processor and the Enclave executable binary to the Attestation Service outside the machine to obtain the resulting data called REPORT signed by the private key held by the Attestation Service, which can use Intel's IAS (Intel Attestation Service) or third party DCAP (Data Center Attestation Primitives).
This REPORT contains a hash value (MRENCLAVE) that depends on the executable binary and execution environment in Enclave, and you can get the same MRENCLAVE by building the same source code with the same configuration.


## TreeKEM

TreeKEM is a Key Encapsulation Mechanism (KEM) to efficiently generate and share group keys among members based on the binary tree structure, and KEM is a mechanism to encrypt common keys using cryptographic primitives such as HPKE (Hybrid Public Key Encryption).
TreeKEM is the core technology of the Ends-to-Ends Encryption (E2EE) protocol for group messaging, known as MLS (Messaging Layer Security), and is being standardized as a draft of the IETF Working Group.

If the key to a particular encrypted message is compromised in a flow where members exchange a series of encrypted messages, Forward Secrecy ensures that the ciphertext before the message is unbreakable, and Post-Compromise Security ensures that the subsequent ciphertext is unbreakable. This means that when communicating between members, a key to a particular ciphertext is compromised to ensure that other ciphertexts are not compromised in a security way. The central technical element in this MLS is TreeKEM, a group key determining mechanism for tree structures. Anonify uses TreeKEM to efficiently share the common group key used by all TEEs without leaking it to the outside world. This allows members to join, leave, and rotate their keys to the group more efficiently than with traditional methods. The result of the state transition encrypted based on this group key is then recorded in the blockchain.
