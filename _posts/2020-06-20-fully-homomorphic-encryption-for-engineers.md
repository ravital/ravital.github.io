---
layout: post
title: "Fully Homomorphic Encryption for Engineers [WIP]"
date: 2020-06-20
---

Fully homomorphic encryption is a fabled technology (at least in the cryptography community) that allows for arbitrary computation over encrypted data. With privacy as a major focus across tech, fully homomorphic encryption (FHE) fits perfectly into the new narrative. FHE is relevant to public, distributed ledgers (such as blockchain) and machine learning.

The 1st FHE scheme was successfully created in 2009 by Craig Gentry. Since then, there have been numerous proposed FHE schemes and a dearth of related work in the space. However, there's little explanation as to *why* all this work is being done or the motivation for any of it. When I first started learning about FHE, I had many questions: Why are there so many schemes? Is FHE being used in any major application right now? If FHE is as powerful as described, why hasn't it taken off yet?

We'll start by providing an introduction to how FHE works and some areas in which it might be useful. The meat of the article, however, is in the 2nd section where we consider why FHE has yet to have received widespread attention and some potential solutions to these issues. Most of these "potential solutions" are current works in progress in academia and industry. Finally, we'll point to some libraries in the space if you're interested in getting your hands dirty.

1. [What is fully homomorphic encryption?](#introduction)
    * [A Refresher on Public Key Encryption Schemes](#section1)
    * [So what's the "homomorphic" part mean?](#section2)
    * [What's needed to build an FHE scheme?](#section3)
    * [Why do we care about any of this?](#section4)
2. [Why FHE hasn't taken off yet](#why)
    * [Many Different Schemes](#section5)
    * [Efficiency](#section6)
    * [Ease of Use](#section7)
3. [Show me the code!](#code)


## What is fully homomorphic encryption? <a name="introduction"></a>
Fully homomorphic encryption is a type of encryption scheme that allows you to perform arbitrary\* computations on encrypted data. 

\* Not completely true but we'll leave that out for now. 

### A Refresher on Public Key Encryption Schemes <a name="section1"></a>
Let's start with a brief review of encryption schemes. *Plaintext* is data in the clear/unecrypted data whereas *ciphertext* is encrypted data.

**Algorithms in an Encryption Scheme**

An encryption scheme includes the following 3 algorithms:
* `KeyGen(security parameter)` &rarr; `(key(s))`: The Key Generation algorithm takes a security parameter as its input. It then outputs the key(s) of the scheme.
* `Encrypt(plaintext, key, randomness)` &rarr;`(ciphertext)`: The Encryption algorithm take a plaintext (i.e. unencrypted data/data in the clear), a key, and some randomness (encryption must be probabilitistic to be "[secure](https://en.wikipedia.org/wiki/Semantic_security)") as inputs. It then outputs the corresponding ciphertext (i.e. encrypted data).
* `Decrypt(ciphertext, key)` &rarr; `(plaintext)`: The Decryption algorithm takes a ciphertext and a key as inputs. It outputs the corresponding plaintext. 

In a *private* key encryption scheme, the same `key` is used to both encrypt and decrypt. That means anyone who can encrypt plaintext (i.e. data in the clear) can also decrypt ciphertext (i.e. encrypted data). These types of encryption schemes are also referred to as "symmetric" since the relationship between encryption/decryption is symmetric. 

However, in a *public* key encryption scheme, different keys are used to encrypt and decrypt. An individual may be able to encrypt data but have no way of decrypting the corresponding ciphertext. These types of encryption schemes are also referred to as "asymmetric" since the relationship between encryption/decryption is asymmetric.

You may have heard of the [ElGamal encryption scheme](https://en.wikipedia.org/wiki/ElGamal_encryption) before (which is based on the [Diffie-Hellman key exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)). These schemes are usually taught in an introductory cryptography course and rely on a commonly used type of cryptography called [elliptic curve cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography). 

### So what's the "homomorphic" part mean? <a name="section2"></a>
We will turn our focus to public key homomorphic encryption schemes as they are the most useful. The homomorphic part implies that there is a special relationship between performing computations in the plaintext space (i.e. all valid plaintexts) vs. ciphertext space (i.e. all valid ciphertexts).

Specifically, in a "homomorphic encryption scheme," the following relationships hold:
* `Encrypt(a) + Encrypt(b) = Encrypt(a + b)`
* `Encrypt(a) * Encrypt(b) = Encrypt(a * b)`

Allowing you to do something like:
`Encrypt(3xy + x) = Encrypt(3xy) + Encrypt(x) = [3 * Encrypt(x) * Encrypt(y)] + Encrypt(x)`

So just by knowing `Encrypt(x)` and `Encrypt(y)`, we can compute the encryption of more complicated polynomial expressions.

### What's needed to build a fully homomorphic encryption scheme? <a name="section3"></a>

All FHE schemes use a specific type of [post-quantum cryptography](https://en.wikipedia.org/wiki/Post-quantum_cryptography) called [lattice cryptography](https://en.wikipedia.org/wiki/Lattice-based_cryptography). The good news is that FHE is resistant to attacks from a quantum computer (if we should be concerned about such a thing is an entirely different topic). The bad news is you might have no familiarity with lattice cryptography even if you've taken an introductory cryptography course before. 

Simply put, lattices sit inside the real vector space (otherwise referred to as R^n) and can be represented using vectors and matrices (which you definitely should have familiarity with!). 

FHE schemes can be broken down into 3 types depending on how they model computation (see [this presentation](http://homomorphicencryption.org/wp-content/uploads/2018/10/CCS-HE-Tutorial-Slides.pdf) if interested in more advanced technical details). The first type models computations as **boolean circuits** (i.e. bits). The second type models computation as **modular arithmetic** (i.e. "clock" arithmetic). The third and final type models computations as **floating point arithmetic**. 

|  Computational Model      |   Good for      |    Schemes\*                                    |  
| ------------------------  | :-------------: |  ------------------------------------------------:|
|           Boolean         |  Number Comparison  |   [TFHE](https://eprint.iacr.org/2016/870.pdf)     |
| Modular Arithmetic        | Integer Arithmetic, Scalar Multiplication   |   [BGV](https://eprint.iacr.org/2011/277.pdf), [BFV](https://eprint.iacr.org/2012/144)      |
| Floating Point Arithmetic  |   Polynomial Approximation, Machine Learning Models  |     [CKKS](https://eprint.iacr.org/2016/421.pdf)          |

The names of these schemes usually come from the last names of the authors who created it. For example, "BGV" comes from the authors Brakerski, Gentry, and Vaikuntanathan.

\* Not an exhaustive list of schemes! I've just listed some well-known ones.

### Why do we care about any of this? <a name="section4"></a>

**Private Transactions on a Public Ledger (e.g. blockchain).** A private transaction scheme allows a user to securely send digital currency to some recipient without revealing the amount being sent to anyone (apart from the intended recipient) on a public, distributed ledger. This is in contrast to cryptocurrencies like Bitcoin where transactions and balances are all public information.

We assume a (public key) fully homomorphic encryption scheme. All encryptions are done with respect to the Receiver's keys. 

1. Sender wants to send `t` tokens to the Receiver. Sender encrypts `t` under the Receiver's public key.
2. Sender publishes `Encrypt(t)` on the public ledger.
3. Notice that `Encrypt(t) + Encrypt(Receiver's balance) = Encrypt(Receiver's balance after performing the transaction)`.

This can be extended to more interesting financial applications such as auctions, voting, and derivatives. An FHE scheme that models computation as *modular arithmetic* is likely to be the most suitable for this type of application.

Note: For this to work in practice, some other cryptographic tools are also needed (such as zero-knowledge proofs). 

**Private Machine Learning.** There's been increased interest in the last few years around how we might be able to "learn" from data (via machine learning) while still keeping the data itself private. FHE is one tool that can help us achieve this goal. In contrast to the previous application, we likely want an FHE scheme that models computations as floating point arithmetic here.

One well-known work in this space is [Crypto-Nets](https://arxiv.org/abs/1412.6181) which combines (as the name might suggest) FHE and [neural networks](https://en.wikipedia.org/wiki/Artificial_neural_network) to allow for outsourced machine learning. When people talk about "outsourced machine learning," the setting usually being described is one in which a data *owner* wants to learn about his data but does not feel comfortable openly sharing his data with a company providing machine learning models. Thus, the data owner encrypts his data using FHE and allows the ML company to run their models on the encrypted data.

Homomorphic encryption is not the sole solution to this problem; [differential privacy](https://en.wikipedia.org/wiki/Differential_privacy#:~:text=Differential%20privacy%20is%20a%20system,about%20individuals%20in%20the%20dataset) is a closely related subject that has garnered a lot of attention in the past few years. Differential privacy is more statistics than cryptography as it does not involve encryption/decryption. It works by adding noise to data in the clear while still guaranteeing that meaningful insights can be drawn from the noisy data sets.  Apple is one major company that [purportedly uses](https://www.apple.com/privacy/docs/Differential_Privacy_Overview.pdf) differential privacy to learn about your behavior while maintaining some level of privacy.


## Why FHE Hasn't Taken off Yet: Challenges in the Space <a name="why"></a>
So far this all sounds great but I've been leaving out a number of problems.

### 1. Many Different Schemes (aka everyone is speaking a different language) <a name="section5"></a>
There a number of different homomorphic encryption schemes (Variety, great!) that offer various tradeoffs depending on the particular use case. Some popular ones include [BGV], [BFV], [CKKS], and [GSW]. 

**Different Models for Computation.** FHE schemes can be broken down into 3 types depending on how they model computation (see [this presentation](http://homomorphicencryption.org/wp-content/uploads/2018/10/CCS-HE-Tutorial-Slides.pdf) if interested in more advanced technical details). The first type models computations as **boolean circuits**. The second type models computation as **modular arithmetic** (i.e. "clock" arithmetic). The third and final type models computations as **floating point arithmetic**. 

|  Computational Model      |   Good for      |    Schemes                                    |  
| :------------------------:|:-------------:|:------------------------------------------------:|
|           Boolean         |  Number Comparison  |   TFHE     |
| Modular Arithmetic        | Integer Arithmetic, Scalar Multiplication   |   &nbsp; BGV, BFV &nbsp;     |
| &nbsp; Floating Point Arithmetic &nbsp;  |  &nbsp; Polynomial Approximation, Machine Learning Models &nbsp;  |     CKKS          |

**Q: Does it matter which scheme I use if it falls under the same category of computational model? (Does it matter which of BGV and BFV I use?)** 

To add further insult to injury&mdash;yes it does matter and it's generally not obvious which one to use when.

**Potential Solutions:**
1. Better resources/guides to explain tradeoffs. Increased benchmarking efforts.
2. Ability to move between different FHE schemes. An academic work on this topic is [CHIMERA](https://eprint.iacr.org/2018/758).


### 2. Efficiency (more complex of an issue than it appears) <a name="section6"></a>
**How many computations do you want to perform?** Some FHE schemes allow you to perform a truly arbitrary number of computations on encrypted data. Many schemes, however, only allow for a certain number of homomorphic operations to be performed (say, for example, we can do 100 sequential homomorphic multiplications). After that point, there's no guarantee decryption will be successful. Say, for example, the FHE scheme only allows for 100 sequential homomorphic multiplications (note: addition is easy in FHE schemes, whereas multiplication is much harder). If we tried to do the 101th homomorphic multiplication, there's no guarantee that the resulting ciphertext can be correctly decrypted. The schemes that allow for a truly arbitrary number of homomorphic computations suffer from very poor performance. If you're able to figure out a ceiling on the number of homomorphic multiplications you want to do, that will help to achieve much better performance.

**How large is your plaintext space?** The larger the plaintext space, the slower it will be to perform operations. 

**What matters most for you in terms of performance?** 

**Potential Solutions:**
1. Hardware acceleration. Some FHE schemes can be accelerated using GPUs; some examples of such libraries include [nuFHE](https://github.com/nucypher/nufhe) and [cuFHE](https://github.com/vernamlab/cuHE). Other efforts to accelerate FHE schemes have included FPGAs (e.g. [HEEAX](https://arxiv.org/pdf/1909.09731.pdf)).


### 3. Ease of Use (there is little, if any, right now) <a name="section7"></a>
Most FHE libraries require deep expertise of the underlying cryptographic scheme to use both correctly and efficiently. Another cryptographer has described working with FHE as similar to writing assembly&mdash; there's a huge difference in performance between good and great implementations.

Wading through libraries like [HElib](https://github.com/homenc/HElib) can be intimidating without a strong background in math/cryptography. 

**Potential Solutions:**
1. Standardization efforts led by the open industry/government/academic consortium ["Homomorphic Encryption Standardization"](https://homomorphicencryption.org/). They've released [draft proposals](https://homomorphicencryption.org/standard/) around security and API standards for homomorphic encryption already.

2. More user-friendly libraries and examples. One example of this is [Microsoft's SEAL library](https://github.com/microsoft/SEAL) which has a incredibly instructive [examples section](https://github.com/microsoft/SEAL/tree/master/native/examples). 

3. Potential compilers. [Cingulata](https://github.com/CEA-LIST/Cingulata) is one such example. 


## Show me the code! <a name="code"></a>

If you do not have previous experience with FHE, we recommend starting with a more user-friendly library such as [SEAL](https://github.com/Microsoft/SEAL).

|  Computational Model                |      Library                                     |  
| :-----------------------------------: |:------------------------------------------------:|
|                    [Boolean](#Many-Different-Schemes)          |  [TFHE](https://github.com/tfhe/tfhe), [nuFHE](https://github.com/nucypher/nufhe), [PALISADE](https://gitlab.com/palisade/palisade-release) (tFHE)   |
| [Modular Arithmetic](#Many-Different-Schemes)                  | &nbsp;  [HElib](https://github.com/homenc/HElib), [SEAL](https://github.com/Microsoft/SEAL) (BFV), [Lattigo](https://github.com/ldsec/lattigo) (BFV), [PALISADE](https://gitlab.com/palisade/palisade-release) (BFV, BGV) &nbsp; |
| &nbsp; [Floating Point Arithmetic](#Many-Different-Schemes) &nbsp;          |  [SEAL](https://github.com/Microsoft/SEAL) (CKKS), [Lattigo](https://github.com/ldsec/lattigo) (CKKS), [PALISADE](https://gitlab.com/palisade/palisade-release) (CKKS)                               |


## References
