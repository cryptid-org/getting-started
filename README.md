<div align="center">
  <a href="https://github.com/cryptid-org">
    <img alt="CryptID" src="docs/img/cryptid-logo.png" width="200">
  </a>
</div>

<div align="center">

</div>

<div align="center">
Cross-platform Identity-based Encryption solution.
</div>

---

# Getting Started with CryptID :rocket:

The aim of this repository is to help you get your head around CryptID, regardless of your previous experience with cryptography.

**Table of Contents**

  * [Identity-based Cryptography](#Identity-based-Cryptography)
    * [Foundations](#Foundations)
    * [Identity-based Encryption](#Identity-based-Encryption)
    * [Further Readings](#Further-Readings)

## Identity-based Cryptography

> Feel free to skip this section, if you know what IBC is. Also, please keep in mind, that this section is just a quick introduction, not an in-depth description.

### Foundations

Identity-based Cryptography (IBC) is a novel branch of [public key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography), where public keys are actual identifiers. To be precise, the public key is a string, clearly identifying an individual or organization in a certain domain.

A domain can be small or mid-scale group (for example, a school or a company) or even the global, worldwide domain.

The choice of identifier is user-dependent: one can employ email addresses as well as telephone numbers or user names. As long as they are unique in the current domain, they are a good fit for Identity-based Cryptography.

How do we benefit from this property? Normally, when using the Public Key Infrastructure (PKI), identities and public keys are bound together using public key certificates. Using IBC, there is no need for such certificates, as (in a somewhat imprecise manner) the public keys are the identities themselves, eliminating the need for certificate authorities.

### Identity-based Encryption

An important application of IBC is Identity-based Encryption (IBE). IBE allows two parties to exchange messages securely while enjoying the benefits of IBC.

The entities taking part in a secure communication are as follows: the sender, the recipient and some trusted third-party. The purpose of this third-party is twofold:

  * It acts as a so-called Private Key Generator (PKG), that can *extract* private keys from public keys.
  * Prior to *extracting* private keys, it handles authentication, ensuring that users are actually identified by their supplied public keys. The actual process of authentication is irrelevant for IBE schemes.

Now, we can consider the communication process itself through the four algorithms of a standard IBE scheme:

  0. **Setup**: Prior to the actual communication, a one-time setup must be performed in order to initialize the system. This process generates the *public parameters* that must be distributed among all parties, and the *master secret* which should only be deployed to the Private Key Generator. The *master secret* allows the PKG to *extract* private keys.
  1. **Encrypt**: The sender uses its identifier to *encrypt* the plaintext message. Afterwards, the ciphertext is transmitted to the recipient.
  2. **Extract**: When the recipient receives the ciphertext, it queries the trusted third-party for its private key with their public key. Given the identity of the recipient is confirmed, the private key is *extracted* and sent back.
  3. **Decrypt**: Possessing the private key pair of the public key, the recipient attemps to *decrypt* the received ciphertext. If the private key matches the public key that was used during encryption, the process succeeds, resulting in the plaintext.

The greatest advantage of IBE lies in the fact that neither the sender, nor the recipient needed to obtain each other's public keys. When performing the encryption, they simply used information they already knew before.

### Further Readings

We advise the interested reader to take a look at the following resources for a more in-depth explanation of IBC and IBE (and their various flavors):

  * [Chatterjee, Sanjit; Sarkar, Palash – Identity-Based Encryption](https://www.springer.com/gp/book/9781441993823)
  * [Martin, Luther – Introduction to Identity-based Encryption](https://dl.acm.org/citation.cfm?id=1370962)
