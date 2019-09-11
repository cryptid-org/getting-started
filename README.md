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
    * [IBC Further Readings](#IBC-Further-Readings)
  * [CryptID](#CryptID)
    * [CrypID.java](#CryptID.java)
    * [CryptID.native + CryptID.js](#CryptID.native-+-CryptID.js)
    * [CryptID Further Readings](#CryptID-Further-Readings)
  * [Maintainers](#Maintainers)
  * [Contributing](#Contributing)
  * [License](#License)
  * [Acknowledgments](#Acknowledgments)

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

### IBC Further Readings

We advise the interested reader to take a look at the following resources for a more in-depth explanation of IBC and IBE (and their various flavors):

  * [Chatterjee, Sanjit; Sarkar, Palash – Identity-Based Encryption](https://www.springer.com/gp/book/9781441993823)
  * [Martin, Luther – Introduction to Identity-based Encryption](https://dl.acm.org/citation.cfm?id=1370962)

## CryptID

CryptID is our IBC implementation based on [RFC 5091](https://tools.ietf.org/html/rfc5091). This RFC describes a flavor of the so-called Boneh-Franklin IBE scheme, which uses techniques from pairing-based cryptography (see [CryptID-Further-Readings](CryptID-Further-Readings)).

In the next section, we will provide detailed information on the novelties of CryptID and its flavors.

### CryptID.java

> The repository can be accessed at: [CryptID.java](https://github.com/cryptid-org/cryptid-java).

A simple Java 8 implementation of the Boneh-Franklin IBE described in RFC 5091.

This flavor of CryptID is not maintained anymore. However, we thought, that given the implementational difficulties regarding pairing-based cryptography, it would still be nice to make it accessible for others. Keep in mind though, that no optimizations were performed, thus, it is not suitable for practical use.

### CryptID.native + CryptID.js

> The repositories can be accessed at: [CryptID.native](https://github.com/cryptid-org/cryptid-native) [CryptID.js](https://github.com/cryptid-org/cryptid-js).

When talking about CryptID, we usually refer to the library formed by these two repositories. The library has two key characteristics:

  * **Cross-platform Operation**
    * The source code of CryptID.native is written in C, which is then compiled to WebAssembly using Emscripten. Although this WebAssembly module could be used as-is, CryptID.js provides an easy-to-use interface on top of that. Together they form a great client-side library, which can be used effectively and securely in WebAssembly-compatible browsers. Of course, server-side embedding is also supported, for example, through Node.js.
  * **Structured Public Key**
    * The key property of IBC is its use of identifiers as public keys. CryptID takes this idea even further, allowing structured documents as public keys. Apart from the identifier itself, these documents may also include any kind of metadata. CryptID employs JSON documents, for example:

      ~~~~JSON
      {
        "instagram": "flashandchill",
        "year": 2019
      }
      ~~~~ 

      Here, the `instagram` field is the actual identifier, while the `year` field is just a piece of metadata: it assigns a validity period of a specific year to the public key. We believe, that public keys like this provide for great flexibility and several interesting domain-specific use cases.

CryptID has a layered structured making it easy for developers for make local changes and optimizations. The five layers are as follows (from bottom to top):

  * **Elliptic-curve Arithmetics** C
    * Elliptic-curve arithmetics routines, currently optimized to Type-1 curves. Big numbers are handled by [GMP](https://gmplib.org/).
  * **Pairing-based Cryptography** C
    * The majority of IBC schemes are based on the pairing operation. In this layer, we have an implementation of the modified Tate-pairing.
  * **Identity-based Cryptography** C
    * The IBC routines themselves, making use of elliptic-curve cryptography and pairings.
  * **Wasm/JavaScript Interoperability** C/JS
    * Serialization/deserialization and conversion between data types.
  * **JavaScript Interface** JS
    * The public interface of the library.

### CryptID Further Readings

If you want to know more about elliptic curves and pairings, then the following resources might come handy:

  * [Handbook of Elliptic and Hyperelliptic Curve Cryptography](https://www.hyperelliptic.org/HEHCC/)
  * [Guide to Pairing-Based Cryptography](https://dl.acm.org/citation.cfm?id=3092800)
  * [Guide to Elliptic Curve Cryptography](https://dl.acm.org/citation.cfm?id=940321)

Regarding WebAssembly, the following links might help:

  * [WebAssembly Official Site](https://webassembly.org/)
  * [Bringing the web up to speed with WebAssembly](https://dl.acm.org/citation.cfm?id=3062363)

## Maintainers

CryptID is maintained by the following team:

  * Ádám Vécsi
    * PhD student, advisor: Dr. Attila Pethő
    * University of Debrecen, Faculty of Informatics (Debrecen, Hungary)
    * vecsi96@gmail.com
    * https://github.com/beardofdoom
  * Attila Bagossy
    * PhD student, advisor: Dr. György Vaszil
    * University of Debrecen, Faculty of Informatics (Debrecen, Hungary)
    * bagossyattila@outlook.com
    * https://github.com/battila7

## Contributing

We are yet to setup a proper Contributing Guide, however, all contributions are welcome in the meantime!

## License

Most parts of CryptID are licensed under the Apache License 2.0. However, please refer to the specific libraries for their appropriate licenses.

## Acknowledgments

This work is supported by the construction EFOP-3.6.3-VEKOP-16-2017-00002. The project is supported by the European Union, co-financed by the European Social Fund.

<p align="right">
  <img alt="CryptID" src="docs/img/szechenyi-logo.jpg" width="350">
</p>
