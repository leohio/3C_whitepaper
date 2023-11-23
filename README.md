Cipher Controller Contract and its application

Leona Hioki, Sora Suegami,(observed by Aayush G)

Abstract:

An encryption architecture that allows decryption based on the execution results of smart contracts on the blockchain. In this paper, we construct the Cipher Controller Contract (3C) using cryptographic primitives, specifically elliptic curve cryptography and BLS Signature-Based Witness Encryption. While smart contracts and blockchain consensus have previously been used solely to determine the cryptographic state within the blockchain, 3C enables control over the decryption of ciphertext existing outside the blockchain. Thus, it can be considered that the functionality of smart contracts has been extended beyond the chain.

Background:

In systems where decryption is conditioned on a signature, there were adapter signatures. In adapter signatures, the process begins with the signer knowing the plaintext, and upon the signature being made public, the plaintext becomes known. Additionally, for reconstructing secrets by the majority of multiple nodes, Shamir's Secret Sharing existed. In SSS, decryption is not done by signatures but by collecting parts of the ciphertext held by each node, meaning the storage of the ciphertext itself serves as key storage, necessitating its distribution.

Subsequently, Signature-Based Witness Encryption was invented as a cryptographic primitive, where the signer, not knowing the plaintext, can restore the ciphertext through threshold signatures without the need to distribute the ciphertext to the signer.

On the other hand, as consensus systems, the BFT system was proposed by Leslie Lamport in 1985, and Nakamoto Consensus by Satoshi Nakamoto in 2008. In 2013, Ethereum, where the execution results of Turing-complete code are determined by Nakamoto Consensus and signatures among multiple nodes, was proposed by Vitalik Buterin and others. Until now, these consensus systems did not perform operations on cryptography outside their system. However, by combining SWE, the background to achieve cryptography decrypted by consensus has been established.

Composition:

While 3C, a ciphertext decrypted based on the consensus of multiple nodes, can theoretically be created in various ways, a composition that combines Turing-complete functionality, sufficient decentralization, and realistic decryption time is considered most promising with the combination of PoS blockchain and SWE.

When using Ethereum, the most secure blockchain under Proof of Stake (PoS), for implementing 3C, there are two methods. The first is a hard fork that allows Beacon chain validators, when assigned as Committee members, to also sign messages from the EVM. The second method involves using re-staking to enable validators capable of BLS signatures to sign new messages. What is common in both cases is that validators sign with their BLS keys not on unpredictable values like block headers, but on values that can be known in advance by the encryptor and are secure.

1. A unique id X is set for the chain.

2. When a 3C smart contract meets specific conditions, it emits the hash value m of the message and X. Additionally, this smart contract can specify who will decrypt the SWE. Someone deploys this.

3. The encryptor specifies the validator set (vs) and threshold, then encrypts the plaintext (p) with the conditional message m using SWE encryption. SWE(vs, m, p).

4. When the conditions are met, the smart contract emits m and the decryptor's address d, and a threshold number of individuals from vs sign m with a BLS signature. Each individual generates a signature BLS(key_i, m).

5. Each individual encrypts BLS(key_i, m) with ElGamal encryption to the decryptor d and adds it to the accumulated encrypted BLS signatures. Σ ElGamal(d, BLS(key_i,m)).

6. The decryptor obtains p by inputting the BLS aggregate signature from the decryption result of Σ ElGamal(d, BLS(key_i,m)) into the SWE ciphertext SWE(vs,m,p).


