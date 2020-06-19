---
layout: post
title: "Fully Homomorphic Encryption for Engineers"
date: 2020-06-20
---

Fully homomorphic encryption is a game-changing technology that allows for computation over encrypted data. It's relevant to public, distributed ledgers (such as blockchain) and machine learning. Yet, from my own experiences talking to engineers, there's a lack of understanding about what FHE is, and why it's relevant to them. Even more importantly, there's a lack of resources explaining *why* FHE hasn't taken off yet if it's as powerful as described. 

We'll start by providing an introduction to how FHE works and some areas in which it might be useful. The meat of the article, however, is in the 2nd section where we consider why FHE has yet to have received widespread attention and some potential solutions to these issues. Finally, we'll point to some libraries in the space if you're interested in getting your hands dirty.

## What is fully homomorphic encryption?
Fully homomorphic encryption is a type of encryption scheme that allows you to perform arbitrary\* computations on encrypted data. 

\* Not completely true but we'll leave that out for now. 

### A Refresher on Public Key Encryption Schemes
Let's start by reviewing encryption schemes briefly. 

**Algorithms in an Encryption Scheme**

An encryption scheme includes the following 3 algorithms:
* `KeyGen(security parameter)` &rarr; `(key(s))`: The Key Generation algorithm takes a security parameter as its input. It then outputs the key(s) of the scheme.
* `Encrypt(plaintext, key, randomness) `&rarr;` (ciphertext)`: The Encryption algorithm take a plaintext (i.e. unencrypted data/data in the clear), a key, and some randomness (encryption must be probabilitistic to be "[secure](https://en.wikipedia.org/wiki/Semantic_security)") as inputs. It then outputs the corresponding ciphertext (i.e. encrypted data).
* `Decrypt(ciphertext, key)` &rarr; `(plaintext)`: The Decryption algorithm takes a ciphertext and a key as inputs. It outputs the corresponding plaintext. 

In a *private* key encryption scheme, the same key is used to encrypt and decrypt. That means anyone who can encrypt plaintext (i.e. data in the clear) can also decrypt ciphertext (i.e. encrypted data). These types of encryption schemes are also referred to as "symmetric" since the relationship between encryption/decryption is symmetric. 

However, in a *public* key encryption scheme, different keys are used to encrypt and decrypt. An individual may be able to encrypt data but have no way of decrypting the corresponding ciphertext. These types of encryption schemes are also referred to as "asymmetric" since the relationship between encryption/decryption is asymmetric.

You may have heard of the [ElGamal encryption scheme](https://en.wikipedia.org/wiki/ElGamal_encryption) before (which is based on the [Diffie-Hellman key exchange](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange)). These schemes are usually taught in an introductory cryptography course and rely on a commonly used type of cryptography called [elliptic curve cryptography](https://en.wikipedia.org/wiki/Elliptic-curve_cryptography). 

### So what's the "homomorphic" part mean?
We will turn our focus to public key encryption schemes as they are the most useful. The homomorphic part implies that there is a special relationship between performing computations in the plaintext space (i.e. all valid plaintexts) vs. ciphertext space (i.e. all valid ciphertexts).

Specifically, in a "homomorphic encryption scheme," the following relationships hold:
* `Encrypt(a) + Encrypt(b) = Encrypt(a + b)`
* `Encrypt(a) * Encrypt(b) = Encrypt(a * b)`

Allowing you to do something like:
`Encrypt(3xy + x) = Encrypt(3xy) + Encrypt(x) = [3 * Encrypt(x) * Encrypt(y)] + Encrypt(x)`

So just by knowing `Encrypt(x)` and `Encrypt(y)`, we can compute the encryption of more complicated polynomial expressions.

### Why do we care about this?

**Examples**

*Private Transactions on a Public Ledger (e.g. blockchain).* A private transaction scheme allows us to securely send digital currency to another without revealing the amount being sent to others (other than the receiver) or know the receiver's balance.

We assume a public key encryption scheme here. All encryptions are done with respect to the Receiver's keys. 

1. Sender wants to send `t` tokens to the Receiver. Sender encrypts `t` under the Receiver's public key.
2. Sender publishes `Encrypt(t)` on the public ledger.
3. Notice that `Encrypt(t) + Encrypt(Receiver's balance) = Encrypt(Receiver's balance after performing the transaction)`.

This can be extended to more interesting financial applications (such as auctions, voting, derivatves).

Note: For this to actually work in practice, some other cryptographic tools are also needed which aren't relevant to our discussion.

*Private Machine Learning.* There's been a increase in interest in the last few years around how we might be able to learn from data (via machine learning) while still maintaining the data's privacy. Homomorphic encryption is not the sole solution to this problem; [differential privacy](https://en.wikipedia.org/wiki/Differential_privacy#:~:text=Differential%20privacy%20is%20a%20system,about%20individuals%20in%20the%20dataset) is a closely related subject.

### What's needed to build a fully homomorphic encryption scheme?

All FHE schemes use a specific type of [post-quantum cryptography](https://en.wikipedia.org/wiki/Post-quantum_cryptography) called [lattice cryptography](https://en.wikipedia.org/wiki/Lattice-based_cryptography). The good news is that FHE is resistant to attacks from a quantum computer (if we should be concerned about such a thing is an entirely different topic). The bad news is you might have no familiarity with lattice cryptography even if you've taken an introductory cryptography course before. 

Simply put, lattices sit inside the real vector space (R^n) and can be represented using vectors and matrices (which you definitely should have familiarity with!). 

## Why FHE Hasn't Taken off Yet: Challenges in the Space
So far this all sounds great but I've been leaving out a number of problems.

### 1. Many Different Schemes (aka everyone is speaking a different language)
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


### 2. Efficiency (more complex of an issue than it appears)
**How many computations do you want to perform?** Some FHE schemes allow you to perform a truly arbitrary number of computations on encrypted data. Many schemes, however, only allow for a certain number of homomorphic operations to be performed (say, for example, we can do 100 sequential homomorphic multiplications). After that point, there's no guarantee decryption will be successful. Say, for example, the FHE scheme only allows for 100 sequential homomorphic multiplications (note: addition is easy in FHE schemes, whereas multiplication is much harder). If we tried to do the 101th homomorphic multiplication, there's no guarantee that the resulting ciphertext can be correctly decrypted. The schemes that allow for a truly arbitrary number of homomorphic computations suffer from very poor performance. If you're able to figure out a ceiling on the number of homomorphic multiplications you want to do, that will help to achieve much better performance.

**How large is your plaintext space?** The larger the plaintext space, the slower it will be to perform operations. 

**What matters most for you in terms of performance?** 

**Potential Solutions:**
1. Hardware acceleration. Some FHE schemes can be accelerated using GPUs; some examples of such libraries include [nuFHE](https://github.com/nucypher/nufhe) and [cuFHE](https://github.com/vernamlab/cuHE). Other efforts to accelerate FHE schemes have included FPGAs (e.g. [HEEAX](https://arxiv.org/pdf/1909.09731.pdf)).


### 3. Ease of Use (honestly, there is none right now)
Most FHE libraries require deep expertise of the underlying cryptographic scheme to use both correctly and efficiently. Another cryptographer has described working with FHE as similar to writing assembly&mdash; there's a huge difference in performance between good and great implementations.

Wading through libraries like [HElib](https://github.com/homenc/HElib) can be intimidating without a strong background in math/cryptography. 

**Potential Solutions:**
1. Standardization efforts led by the open industry/government/academic consortium ["Homomorphic Encryption Standardization"](https://homomorphicencryption.org/). They've released [draft proposals](https://homomorphicencryption.org/standard/) around security and API standards for homomorphic encryption already.

2. More user-friendly libraries and examples. One example of this is [Microsoft's SEAL library](https://github.com/microsoft/SEAL) which has a incredibly instructive [examples section](https://github.com/microsoft/SEAL/tree/master/native/examples). 

3. Potential compilers. [Cingulata](https://github.com/CEA-LIST/Cingulata) is one such example. 


## Show me the code!

If you do not have previous experience with FHE, we recommend starting with a more user-friendly library such as [SEAL](https://github.com/Microsoft/SEAL).

|  Computational Model                |      Library                                     |  
| :-----------------------------------: |:------------------------------------------------:|
|                    [Boolean](#Many-Different-Schemes)          |  [TFHE](https://github.com/tfhe/tfhe), [nuFHE](https://github.com/nucypher/nufhe), [PALISADE](https://gitlab.com/palisade/palisade-release) (tFHE)   |
| [Modular Arithmetic](#Many-Different-Schemes)                  | &nbsp;  [HElib](https://github.com/homenc/HElib), [SEAL](https://github.com/Microsoft/SEAL) (BFV), [Lattigo](https://github.com/ldsec/lattigo) (BFV), [PALISADE](https://gitlab.com/palisade/palisade-release) (BFV, BGV) &nbsp; |
| &nbsp; [Floating Point Arithmetic](#Many-Different-Schemes) &nbsp;          |  [SEAL](https://github.com/Microsoft/SEAL) (CKKS), [Lattigo](https://github.com/ldsec/lattigo) (CKKS), [PALISADE](https://gitlab.com/palisade/palisade-release) (CKKS)                               |


## References
