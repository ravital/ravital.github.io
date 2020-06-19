---
layout: post
title: "Resources for Lattice Cryptography, Fully Homomorphic Encryption, and Zero-knowledge Proofs"
date: 2020-06-19
---
I've collected some resources if you're interested in learning more about lattice-based cryptography, fully homomorphic encryption (FHE), or zero-knowledge proofs (ZKP). Wikipedia is a great place to start before diving into anything listed below. 

## Lattice-based Cryptography
Lattice-based cryptography is a type of [post-quantum crytography](https://en.wikipedia.org/wiki/Post-quantum_cryptography)(i.e. cryptography that is resistant to attacks from a quantum computer) using [lattices](https://en.wikipedia.org/wiki/Lattice_(group)).

- *[An Introduction to Mathematical Cryptography](https://link.springer.com/book/10.1007/978-1-4939-1711-2)*. See the "Lattices and Cryptography" chapter. This was the first resource I used in undergrad to learn about lattice-based cryptography.
- [A Decade of Lattice Cryptography](https://web.eecs.umich.edu/~cpeikert/pubs/lattice-survey.pdf). A lot more advanced than the previous resource but thoroughly covers the major aspects of lattice-based cryptography.
- [Simons Institute Lattices Program](https://simons.berkeley.edu/programs/lattices2020). Videos covering everything from the basics of lattices to current research being done today.

## Fully Homomorphic Encryption
Fully homomorphic encryption can be viewed as an extension of [public key encryption schemes](https://en.wikipedia.org/wiki/Public-key_cryptography). It allows for arbitrary computations over encrypted data in a "structure-preserving" way.

- [A Brief Survey of Fully Homomorphic Encryption](https://blog.quarkslab.com/a-brief-survey-of-fully-homomorphic-encryption-computing-on-encrypted-data.html). A short post introducting what fully homomorphic encryption is and how it works. A good starting point if you don't know anything about FHE.
- [Computing Arbitrary Functions of Encrypted Data](https://crypto.stanford.edu/craig/easy-fhe.pdf). A brief but very well-written paper from the Craig Gentry (the 1st person to successfully construct an FHE scheme). More technically involved than the above but provides good intuition for understanding how FHE works.
- [Fundamentals of Fully Homomorphic Encryption](https://pdfs.semanticscholar.org/e247/ae732c50b6c04b2aa413c4caa0ca77ed4751.pdf). An accessible but longer introduction to FHE.
- [Microsoft SEAL Examples](https://github.com/microsoft/SEAL/tree/master/native/examples). Allows you to see how FHE works in practice. Does not assume prior knowledge of cryptography. Probably the most useful resource if you're an engineer.

## Zero-knowledge Proofs
Zero-knowledge proofs allow you to convince someone (called a Verifier) of some fact without "revealing" any additional information (hence the term "zero-knowledge" because the Verifier learned nothing after the interaction apart from being convinced that the fact was true).

- [Zero Knowledge Proofs: An illustrated primer](https://blog.cryptographyengineering.com/2014/11/27/zero-knowledge-proofs-illustrated-primer/). Fantastic blog post explaining ZKPs. A good starting point.
- *[Cryptography: An Introduction](https://www.cs.umd.edu/~waa/414-F11/IntroToCrypto.pdf)*. See the "Zero-Knowledge Proofs" chapter.
- [Simons Institute Proofs, Consensus and Decentralizing Society Bootcamp](https://simons.berkeley.edu/workshops/schedule/9299). Specifically the videos from days 1 and 2.
- [ZKProof Community Reference](https://docs.zkproof.org/pages/reference/reference.pdf). A very long reference document explaining various technical aspects of ZKPs from [ZKProof](https://zkproof.org/), an intiative to standardize ZKPs. This document is good to look at if you encounter a term you don't know.
