---
layout: post
title: "Resources for Lattice Cryptography, Fully Homomorphic Encryption, and Zero-knowledge Proofs"
date: 2020-06-20
---
I've collected some resources if you're interested in learning more about lattice-based cryptography, fully homomorphic encryption (FHE), or zero-knowledge proofs (ZKP). Wikipedia is a great place to start before diving into anything listed below. 

## Lattice-based Cryptography
- *[An Introduction to Mathematical Cryptography](https://link.springer.com/book/10.1007/978-1-4939-1711-2)*, see the "Lattices and Cryptography" chapter. This was the first resource I used in undergrad to learn about lattice-based cryptography.
- [A Decade of Lattice Cryptography](https://web.eecs.umich.edu/~cpeikert/pubs/lattice-survey.pdf). A lot more advanced than the previous resource but thoroughly covers the major aspects of lattice-based cryptography.
- [Simons Institute Lattices Program](https://simons.berkeley.edu/programs/lattices2020). Videos covering everything from the basics of lattices to current research being done today.

## Fully Homomorphic Encryption
- [A Brief Survey of Fully Homomorphic Encryption](https://blog.quarkslab.com/a-brief-survey-of-fully-homomorphic-encryption-computing-on-encrypted-data.html). A short post introducting what fully homomorphic encryption is and how it works. A good starting point if you don't know anything about FHE.
- [Computing Arbitrary Functions of Encrypted Data](https://crypto.stanford.edu/craig/easy-fhe.pdf). A brief but very well-written paper from the Craig Gentry (the 1st person to successfully construct an FHE scheme). More technically involved than the above but provides good intuition for understanding how FHE works.
- [Fundamentals of Fully Homomorphic Encryption](https://pdfs.semanticscholar.org/e247/ae732c50b6c04b2aa413c4caa0ca77ed4751.pdf). An accessible but longer introduction to FHE.
- [Microsoft SEAL Examples](https://github.com/microsoft/SEAL/tree/master/native/examples). Allows you to see how FHE works in practice. Does not assume prior knowledge of cryptography. 

## Zero-knowledge Proofs
- [Zero Knowledge Proofs: An illustrated primer](https://blog.cryptographyengineering.com/2014/11/27/zero-knowledge-proofs-illustrated-primer/). Fantastic blog post explaining ZKPs. A good starting point.
- *[Cryptography: An Introduction](https://www.cs.umd.edu/~waa/414-F11/IntroToCrypto.pdf)*, see the "Zero-Knowledge Proofs" chapter.
- [ZKProof Community Reference](https://docs.zkproof.org/pages/reference/reference.pdf). A very long reference document explaining various technical aspects of ZKPs from [ZKProof](https://zkproof.org/), an intiative to standardize ZKPs.
