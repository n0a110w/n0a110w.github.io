---
layout: default
title: Cryptography
nav_order: 2 
parent: security stuff
---

cryptography notes

1. DES/3DES
2. AES
3. RC4, RC5 and RC6
4. twofish
  - Symmetric key block cipher
  - block size: 128 bits
  - Key size: up to 256 bits
  - Key-dependent S-Boxes: obscure relationship of key and cipher
5. DSA [(Digital Signature Algorithm)](https://en.wikipedia.org/wiki/Digital_Signature_Algorithm)
  - 2 phases of key generation: 
    - choice of algorithm paramaters (shared between different users)
    - computes public and private keys for users
    - ECDSA (Elliptic Curve Signature Algorithm) [(Remember the Sony PS3 hack)](https://www.mydigitallife.net/fail0verflow-hack-permanent-sony-ps3-crack-to-code-sign-homebrew-games-and-apps/)
6. RSA [(Rivest Shamir Adleman)](https://en.wikipedia.org/wiki/Rivest%E2%80%93Shamir%E2%80%93Adleman_cryptosystem)
  - Asymmetric
  - Each user has his/her own private key. Public keys are shared. 
7. Diffie-Hellman [(DH)](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)
  - [*...a way of generating a shared secret between two people in such a way that the secret can't be seen by observing the communication...*](https://security.stackexchange.com/questions/45963/diffie-hellman-key-exchange-in-plain-english)
  - Alice and Bob [more info](https://www.wst.space/ssl-part-2-diffie-hellman-key-exchange/)
8. MD5, SHA, RIPEMD-160, HMAC
- MD5 (Message Digest Function)
  - 128-bit value
  - non-identical messages can have the same hash value (collision!!)
- SHA (Secure Hashing Algorithm)
  - SHA-1: 160-bit hash value
  - SHA-2: 224, 256, 384 and 512 bit 
  - SHA-3: 512 bit value, sponge construction
- RIPEMD-160 (RACE Integrity Primitives Evaluation Message Digest)
  - 160-bit
- HMAC (Hash-based Message Authentication Code)
  - cryptographic hash function and cryptogrpahic key
  - integrity and authentication
  - can be used with other algorithms: `HMAC_MD5("key","the quick brown fox jumps over the lazy dog")`

Related Tools:  
- [HashCalc](http://www.slavasoft.com/hashcalc/)
- [online md5 decryption tool](https://md5decrypt.net/en/)


---

10. PKI [(Public Key Infrastructure)](https://en.wikipedia.org/wiki/Public_key_infrastructure)
- roles, policies and procedures needed to create, manage, distribute, use, store and revoke digital certificates and manage public key encryption
- bind public keys to entities
- CA (Certificate Authority) 
- RA (Regisration Authority)
- Web of trust (self-signed)

---

### Email Encryption
- DSA (Digital Signature Algorithm) 
- SSL (Secure Sockets Layer): See ["Poodle"](https://security.stackexchange.com/questions/70719/ssl3-poodle-vulnerability) Vulnerability
- TLS (Transport Layer Security)
  - TLS 1.2
    - SHA-256
    - Removed SSL capability
  - TLS 1.3
   - Removed support for MD5, SHA-224, weak elliptic curves
   - [Every bytes explained and reproduced](https://tls13.ulfheim.net/)
- PGP [(Pretty Good Privacy)](https://en.wikipedia.org/wiki/Pretty_Good_Privacy)
  - End-to-end encryption
  - OpenPGP Standard (RFC 4880)
  - Hashing, Data Compression, Symmetric, Assymetric

Related Tools:  	
- [OpenSSL](https://www.openssl.org/) 
- [keyczar](https://github.com/google/keyczar)
- [GnuPG](https://gnupg.org/>)

---

### Attacks
1. Brute-Force: passwords/passphrases
2. Birthday
3. Meet-in-the-Middle: space-time tradeoff
4. DUHK [(Don't Use Hard-coded Keys)](https://duhkattack.com/)
5. Rainbow Table 


