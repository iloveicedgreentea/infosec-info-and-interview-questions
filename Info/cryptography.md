# Cryptography

## Acronyms you should know

These are not in any particular order

* RSA - Rivest–Shamir–Adleman. This is an asymmetric algorithm used for encryption and signatures. Very popular and used pretty much everywhere. SSH, TLS, you name it.
* AES - Advanced Encryption Standard. This is the *implementation standard* of `Rijndael`, not the actual cipher. This is a symmetric cipher. Everyone uses this. Only valid key sizes are 128, 192, and 256. Block size is always 128 bits. 
* DH - Diffie–Hellman. This is a key exchange algorithm such that two or more parties can derive a shared secret without actually exchanging secret info. They share some public data, do some logarithmic magic, and get a shared secret. This shared secret is a symmetric key for encryption, generally using AES. [Authentication during exchange is a must](cryptography.md#diffie-hellman-authentication). The `public data` is referred to as the `DH parameters`.  These must be random, but lengths are pre-defined (Group 2, 5, etc)
* DHE - Diffie-Hellman Ephemeral. Similar to DH but the parties agree to regenerate the session key after use. This is part of the concept of `Perfect Forward Secrecy`.[ If someone collects a whole bunch of HTTPS sessions and cracks a static key,](https://en.wikipedia.org/wiki/Utah_Data_Center) which the NSA is totally not doing at all, then all of those sessions are cracked. DHE limits it to a single session.
* ECCDHE - Like DHE, but instead of using extremly complicated modulus math, it uses extremely complicated eliptic curve cryptography. Very cool, and very legal ([Until the FBI has its way](https://en.wikipedia.org/wiki/FBI%E2%80%93Apple_encryption_dispute)).
* EdDSA - Edwards-curve Digital Signature Algorithm. This is an ECC signature algorithm using Curve25519. Curve25519 is a popular alternative to NIST's P-XXX curves which are not backdoored, [according to the people who have the technical means to backdoor it](https://en.wikipedia.org/wiki/Dual_EC_DRBG).
* MITM - Man in the middle. This is a common attack where someone intercepts your communication and eavesdrops or changes it. Instead of A<->B, you have A<->C<->B and neither party will know. Safeguards include TLS, PKI, anything that both encrypts AND authenticates the communication. 



## Concepts you should know

* Asymmetric keys - Think RSA. Public and private key pair. Public used to encrypt/verify signature. Private used to sign/decrypt. Public can be shared without limits. Signature is proof of ownership.
* Symmetric Keys - One key, so cannot be shared publicly. Used in standards like AES. Much faster than Asymmetric keys for encryption. 
* Certificate - This is a public key container that is signed by an entity like a Root CA. Has info like public key, host name, etc
* Perfect Forward Secrecy - 

Envelope Encryption

* Protecting data keys When you encrypt a data key, you don't have to worry about storing the encrypted data key, because the data key is inherently protected by encryption. You can safely store the encrypted data key alongside the encrypted data.  
* Encrypting the same data under multiple master keys Encryption operations can be time consuming, particularly when the data being encrypted are large objects. Instead of re-encrypting raw data multiple times with different keys, you can re-encrypt only the data keys that protect the raw data.  
* Combining the strengths of multiple algorithms In general, symmetric key algorithms are faster and produce smaller ciphertexts than public key algorithms. But public key algorithms provide inherent separation of roles and easier key management. Envelope encryption lets you combine the strengths of each strategy. 






## Diffie-Hellman Authentication
DH algorithms (DH, ECCDHE, etc) are susceptible to MITM. While no secrets are shared per se, an attacker can agree on a shared secret with both parties - A<->C<->B - while A and B think they are talking to each other. The To combat this, you need to sign the DH parameters with a private key. Assuming the private key has not been stolen or [cracked](https://en.wikipedia.org/wiki/Shor%27s_algorithm), it proves those parameters have not been forged and the key exchange is secure, even if someone is eavesdropping on the conversation. Remember, someone can see all of the DH parameters and still not be able to generate the session key. This is the magic of DH. 

## TLS

Putting all the above in context, we can finally start to understand TLS. Here is how it works at a high level: 

### RSA

* A client and server start a conversation. 
    * Client Hello - Client sends TLS version, cipher suites supported, and a` client random`
    * Server Hello - Server send TLS certificate, the cipher suite and tls version to use, and a `server random`
* Authentication
    * Client checks if it trusts the certificate. This verifies the server.
* Premaster secret
    * The client generates a random `premaster secret` encrypted with the public key from the TLS certificate and sends it to server.
    * Server decrypts it since it has the private key which matches the public key in the certificate.
* Secret key generation
    * Both parties perform fancy math on both randoms and the premaster key. This results in the `master secret` used to derive the key for encryption. 
* Both parties inform each other they are ready and the handshake is done. 

### DH

* A client and server start a conversation. 
    * Client Hello - Client sends TLS version, cipher suites supported, and a` client random`
    * Server Hello - Server send TLS certificate, the cipher suite and tls version to use, and a `server random`
* Authentication
    * The server encrypts the client random, the server random, and the DH parameters. 
    * Client decrypts said information and sends its DH parameters to server
* Secret key generation
    * Premaster secret is generated from the DH parameters thanks to DH, without exchanging secrets.
    * Both parties perform fancy math on both randoms and the premaster key. This results in the `master secret` used to derive the key for encryption. 
* Both parties inform each other they are ready and the handshake is done. 

Communication with TLS also has integrity which is done with MAC/HMAC. `cipher suites` include hashing algorithms which are used for HMACs. A simple explaination is if you have a message, you can hash the message and the shared key together. Since both parties have the key, once the message is decrypted, a client can hash that message with the key and if it matches the HMAC it received, it knows that the message was not modified. An attacker can modify the message but if they do not have the key, they cannot forge the HMAC. 

MACs also contain a sequence number to prevent a [replay attack](attacks.md#replay-attack)

