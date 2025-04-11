+++
title = "Cryptography"
date = 2024-05-01

[taxonomies]
tags=["cryptography"]
+++

# Cryptography Notes/Resources

## Non-Technical
- [Aggregated Blockchains: A New Thesis](https://polygon.technology/blog/aggregated-blockchains-a-new-thesis) → Polygon feb 24
- [Electronification, Trading, and Crypto](https://blog.uniswap.org/electronification-trading-and-crypto) → Uniswap → Decentralization.
- [A Cambrian Explosion of Crypto Proofs](https://nakamoto.com/cambrian-explosion-of-crypto-proofs/)
    - [Symmetric Encryption is Quantum proof?](https://en.m.wikipedia.org/wiki/Post-quantum_cryptography)
    - Takeaway: asymmetric circuit-specific systems (Groth16) are shortest, shorter than all asymmetric universal ones, and all symmetric systems.
    - Post Quantum Commitment Scheme → Merkle Trees.
- [A Brief History of Money](https://nakamoto.com/a-brief-history-of-money/)
    - A medium of exchange is the asset we use to directly settle transactions. This is the easiest hurdle to clear. You can use Starbucks rewards points to buy a latte, so Starbucks points function as a medium of exchange. But of course, Starbucks points aren't a great store of value—people know this instinctively and don't store their savings into Starbucks points. This is not just because it's impractical; people are aware that Starbucks might modify their rewards program to devalue these points, and there's not a stable market for selling saved up points. → The US dollar can also be devaluated by some “Starbucks”.
    - Money is just a bubble that never pops.
- [The Cypherpunks](https://nakamoto.com/the-cypherpunks/)
- [Satoshi Nakamoto](https://nakamoto.com/satoshi-nakamoto/)
- [SNARK proving ASIC](https://twitter.com/drakefjustin/status/1755929540700807211)

## Technical

- [Elliptic Curve ZK-Proof Acceleration on AMD Versal](https://www.ulvetanna.io/news/elliptic-curve-acceleration-amd-versal)
    - [AMD XDNA: Meet 2023 ZK Acceleration King | by Ingonyama | Medium](https://medium.com/@ingonyama/amd-xdna-meet-2023-zk-acceleration-king-323a29c93b75)
- ["A deep dive into optimizing Multi-Scalar Multiplication" (Niall Emmart, Yrrid) - YouTube](https://www.youtube.com/watch?v=KAWlySN7Hm8)
- [math - How could one implement multiplication in finite fields? - Stack Overflow](https://stackoverflow.com/questions/1935658/how-could-one-implement-multiplication-in-finite-fields)
- [Galois Field New Instructions (GFNI) Technology Guide](https://networkbuilders.intel.com/solutionslibrary/galois-field-new-instructions-gfni-technology-guide) → Faster multiplication with these instructions?  [Discovering novel algorithms with AlphaTensor - Google DeepMind](https://deepmind.google/discover/blog/discovering-novel-algorithms-with-alphatensor/)
- [Hash Functions](https://nakamoto.com/hash-functions/)
- [Merkle Trees](https://nakamoto.com/merkle-trees/) — [Merkle Trees paper](https://web.archive.org/web/20141222120036/http://www.emsec.rub.de/media/crypto/attachments/files/2011/04/becker_1.pdf)
  - [Solidity: Merkle Trees, BitMaps & Coding an Airdrop - YouTube](https://www.youtube.com/watch?v=Iv0cPT-7AR8)
  - [Learn Solidity (0.5) - Merkle Tree - YouTube](https://www.youtube.com/watch?v=n6nEPaE7KZ8)
  - [Using Merkle Trees for Smart Contracts | by mbvissers.eth | CodeX | Medium](https://medium.com/codex/using-merkle-trees-for-smart-contracts-24ccf6f75a0a)
- [Hashcash](https://nakamoto.com/hashcash/)
- [Public-Key Cryptography](https://nakamoto.com/public-key-cryptography/) 
    - ECC > RSA
- [Zero Knowledge Proof - YouTube](https://www.youtube.com/playlist?list=PLO5VPQH6OWdWPjQo_bxy8Oc1NFShta83y) → How Tornado Works? → Practical use case of ZKP.
- [Dual_EC_DRBG - Wikipedia](https://en.wikipedia.org/wiki/Dual_EC_DRBG) → Elliptic Curves’ backdoor.
- https://github.com/lambdaclass/cairo-vm?tab=readme-ov-file#computational-integrity-and-zero-knowledge-proofs
- [Crypto101](https://www.crypto101.io/)
- [Eth2Book](https://eth2book.info/capella/contents/)
- [FrontRunning](https://coinmarketcap.com/academy/es/glossary/front-running)
- [How to Design Schnorr Signatures - YouTube](https://www.youtube.com/watch?v=wjACBRJDfxc)
- [RCIG_Coordination_Repo](https://github.com/The-DevX-Initiative/RCIG_Coordination_Repo)
- [Extended Euclidean Algorithm](https://web.archive.org/web/20230511143526/http://www-math.ucdenver.edu/~wcherowi/courses/m5410/exeucalg.html)
- [Why do we need in RSA the modulus to be product of 2 primes? - Cryptography Stack Exchange](https://crypto.stackexchange.com/questions/5170/why-do-we-need-in-rsa-the-modulus-to-be-product-of-2-primes)
- [Textbook RSA with exponent e=3 - Cryptography Stack Exchange](https://crypto.stackexchange.com/questions/18301/textbook-rsa-with-exponent-e-3)

## ZK-Proofs:

[Ethereum’s ZK rollups:](https://ethereum.org/developers/docs/scaling/zk-rollups)

- **Proof generation and verification**: ZK-rollup operators must produce validity proofs for transaction batches, which is resource-intensive. Verifying zero-knowledge proofs on Mainnet also costs gas (~ 500,000 gas).
- An advantage of zero-knowledge proofs is that proofs can verify other proofs. For example, a single ZK-SNARK can verify other ZK-SNARKs. Such "proof-of-proofs" are called recursive proofs and dramatically increase throughput on ZK-rollups.
- ⚠️Producing validity proofs requires specialized hardware, which may encourage centralized control of the chain by a few parties.

## Bootcamp:

- [How arithmetic circuits are used to verify zero knowledge proofs](https://www.rareskills.io/post/zk-circuits)
- [The Zero Knowledge Blog](https://www.zeroknowledgeblog.com/)
- [Square Span Programs with Applications to Succinct NIZK Arguments](https://eprint.iacr.org/2014/718)
- [718.pdf](https://eprint.iacr.org/2014/718.pdf)
- [260.pdf](https://eprint.iacr.org/2016/260.pdf)
- [Survey-SNARKs.pdf](https://www.di.ens.fr/~nitulesc/files/Survey-SNARKs.pdf)
- [babySNARK](https://github.com/initc3/babySNARK)
- [programmers-introduction-to-mathematics](https://github.com/pim-book/programmers-introduction-to-mathematics)
- [Programming with Finite Fields – Math ∩ Programming](https://jeremykun.com/2014/03/13/programming-with-finite-fields/)
- [Quadratic Arithmetic Programs: from Zero to Hero | by Vitalik Buterin | Medium](https://medium.com/@VitalikButerin/quadratic-arithmetic-programs-from-zero-to-hero-f6d558cea649)
- [Zk-SNARKs: Under the Hood. This is the third part of a series of… | by Vitalik Buterin | Medium](https://medium.com/@VitalikButerin/zk-snarks-under-the-hood-b33151a013f6)
- [[1906.07221] Why and How zk-SNARK Works](https://arxiv.org/abs/1906.07221)
- [Cryptohack](https://cryptohack.org/)
- [Zero Knowledge Proofs for Kids](https://eli5.zksync.io/glossary)
- [ZK Book | RareSkills](https://www.rareskills.io/zk-book)

## University Courses
- [Crypto Stanford](https://crypto.stanford.edu/~dabo/courses/OnlineCrypto/)
- [ZK MIT course](https://zkiap.com/)
