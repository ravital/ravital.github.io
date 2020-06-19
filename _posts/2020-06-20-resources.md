---
layout: post
title: "Resources for Lattice Cryptography, Fully Homomorphic Encryption, and Zero-knowledge Proofs"
date: 2020-06-20
---
I've collected some resources if you're interested in learning more about lattice-based cryptography, fully homomorphic encryption (FHE), or zero-knowledge proofs (ZKP). Wikipedia is a great place to start before diving into anything listed below. 

## Lattice-based Cryptography
- *[An Introduction to Mathematical Cryptography](https://link.springer.com/book/10.1007/978-1-4939-1711-2)*, see the "Lattices and Cryptography" chapter. This was the first resource I used in undergrad to learn about lattice-based cryptography.
- [A Decade of Lattice Cryptography](https://web.eecs.umich.edu/~cpeikert/pubs/lattice-survey.pdf). A lot more advanced than the previous resource but thoroughly covers the major aspects of lattice-based cryptography.

## Fully Homomorphic Encryption
- [A Brief Survey of Fully Homomorphic Encryption](https://blog.quarkslab.com/a-brief-survey-of-fully-homomorphic-encryption-computing-on-encrypted-data.html). An easy introduction to understanding what fully homomorphic encryption is and how it works.
- [Computing Arbitrary Functions of Encrypted Data](https://crypto.stanford.edu/craig/easy-fhe.pdf). A brief but very well-written paper from the Craig Gentry (the 1st person to successfully construct an FHE scheme). More technically involved than the above but provides good intuition for understanding how FHE works.
- [Fundamentals of Fully Homomorphic Encryption](https://pdfs.semanticscholar.org/e247/ae732c50b6c04b2aa413c4caa0ca77ed4751.pdf). An accessible but longer introduction to FHE.
- [Microsoft SEAL Examples](https://github.com/microsoft/SEAL/tree/master/native/examples). Allows you to see how FHE works in practice. Does not assume prior knowledge of cryptography (much less FHE). 

## Zero-knowledge Proofs
