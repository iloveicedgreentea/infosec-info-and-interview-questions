# Cryptography

## Acronyms you should know

These are not in any particular order

* RSA - Rivest–Shamir–Adleman. This is an asymmetric algorithm used for encryption and signatures. Very popular and used pretty much everywhere. SSH, TLS, you name it.
* AES - Advanced Encryption Standard. This is the *implementation standard* of `Rijndael`, not the actual cipher. This is a symmetric cipher. Everyone uses this. Only valid key sizes are 128, 192, and 256. Block size is always 128 bits. 
* DH - Diffie–Hellman. This is a key exchange algorithm such that two or more parties can derive a shared secret without actually exchanging secret info. They share some public data, do some logarithmic magic, and get a shared secret. This shared secret is a symmetric key for encryption, generally using AES. [Authentication during exchange is a must](cryptography.md#diffie-hellman-authentication). The `public data` is referred to as the `DH parameters`. 
* DHE - Diffie-Hellman Ephemeral. Similar to DH but the parties agree to regenerate the session key after use. This is part of the concept of `Perfect Forward Secrecy`.[ If someone collects a whole bunch of HTTPS sessions and cracks a static key,](https://en.wikipedia.org/wiki/Utah_Data_Center) which the NSA is totally not doing at all, then all of those sessions are cracked. DHE limits it to a single session.
* ECCDHE - Like DHE, but instead of using extremly complicated modulus math, it uses extremely complicated eliptic curve cryptography. Very cool, and very legal ([Until the FBI has its way](https://en.wikipedia.org/wiki/FBI%E2%80%93Apple_encryption_dispute)).
* MITM - Man in the middle. This is a common attack where someone intercepts your communication and eavesdrops or changes it. Instead of A<->B, you have A<->C<->B and neither party will know. Safeguards include TLS, PKI, anything that both encrypts AND authenticates the communication. 

## Concepts you should know

* Asymmetric keys - Think RSA
* Symmetric Keys

* Perfect Forward Secrecy - 








## Diffie-Hellman Authentication
DH algorithms (DH, ECCDHE, etc) are susceptible to MITM. While no secrets are shared per se, an attacker can agree on a shared secret with both parties - A<->C<->B - while A and B think they are talking to each other. The To combat this, you need to sign the DH parameters with a private key. Assuming the private key has not been stolen or [cracked](https://en.wikipedia.org/wiki/Shor%27s_algorithm), it proves those parameters have not been forged and the key exchange is secure, even if someone is eavesdropping on the conversation. Remember, someone can see all of the DH parameters and still not be able to generate the session key. This is the magic of DH. 