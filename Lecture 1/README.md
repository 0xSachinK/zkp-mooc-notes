# Introduction and History of ZKP

Lecture:[ ZKP MOOC Lecture 1: Introduction and History of ZKP](https://youtu.be/uchjTIlPzFo?si=2SBNSyeecfhKwpF3) |
Author of Notes: [0xSachinK](https://twitter.com/0xSachinK)

## Classical proofs

For a Claim x.

> Prover -----w-----> Verifier (accept/reject)

|w| = polynomial in |x|
Accepts x if V(x,w) = 1 else rejects.

In this lecture, prover is unbounded, and all the attention is given to how much time it takes to verify.

Examples of classical proofs:

1. N is a product of 2 large primes:
   - After interaction V knows:
     - N is product of 2 primes
     - The two primes p and q
   - Verifier multiplies and makes sure N = p \* q
2. y is a quadratic residue mod N (y = x\*\*2 mod N)
   - After interaction V knows:
     - y is a qaudratic resideue mod
     - Square root of y
3. Graphs are isomporphic
   - After interaction V knows:
     - G0 is isomorphic to G1
     - The isomporphism _pi_

## NP Language

A language L is set of binary strings x.

![](https://hackmd.io/_uploads/S1Wthg3oh.png)

## Zero Knowledge Interactive Proofs

- Interaction: More than a single message between P and V
- Randomness: Verifier is randomized and his questions to the verifier are unpredictable

### Interactive Proofs

Interactive proofs for a language L
![](https://hackmd.io/_uploads/S1ydJbnsn.png)

Definition: Class of languages IP = {L for which there is an interactive proof}

### What is Zero Knowledge?

For true statements, _for every verifier,_
What the verifier can compute after the interaction = What the verifier could have computed before interaction.

#### Computational Indistinguishability

If no polynomial time "distinguisher" can tell apart two different probability distributions they are "effectively the same".

                        \    D1 K - bit string
    Distinguisher  <----\
                        \    D2 K - bit string

![](https://hackmd.io/_uploads/BkSQCW3i3.png)

- The squiggly equals means computationaly indistinguishable.

> (P,V) is a zero-knowledge interactive protocol if it is complete, sound and zero-knowledge.

## Proof of Knowledge

![](https://hackmd.io/_uploads/H1qFMz3o3.png)

- Extractor has access to the prover as an oracle.
- In the square root example:
  - The extractor would first ask for r.
  - Then rewinds.
  - The extractor asks for rx.
  - The extractor then calculate x = rx/r mod N.
  - Thus there exists a polynomial time extractor that can extract x from prover if the prover knew the answer.

## All of NP is Zero Knowledge

**Do all NP languages have Zero Knowledge Proofs?**

> Due to NP completeness reducability, if a language is NP complete, then every string x can be reduced to a graph G(x), such that, if x is in the language then the graph is 3-colorable and if x is not in the language then the graph is not 3-colorable. That is every NP complete problem can be reduced to the 3-colorable problem.

So in order to profo that all of NP is ZKP, you just prove that 3-colorable is ZKP.
Note: To prove 3-colorable is ZKP, we need one way hash functions that have "hiding" and "binding" properties.

### 3-colorable is ZKP

1. Prover: Picks a random permutation of colors (R, G, B), colors the graph and commits to it.
2. Verifier: Select a random edge e = (a, b) to send to Prover.
3. Prover: Decommit colors for vertex a and b.

Decision: Verifier rejects if color(a) != color(b), otherwise Verifier repeats steps 1-3 and accepts after k iterations.

**Completeness**: If G is 3-colorable, then the honest prover uses a proper 3-coloring & the verifier always accept.

**Soundness**: If G is not 3-colorable, then for all P\*, Prob(Verifier accepts) < 1 - 1/|E|^k, which vanishes with a very high k

**Zero Knowledge**: Easy to see informally, that the verifier doesn't learn anything.

## Fiat-Shamir paradigm

Instead of verifier genearting randomness. Prover generates randomness by hashing the previous transcript. If the hash function does behave like a random-oracle, then completeness and soundness holds.

**What if first message is from the verifier?**

> Idea (used in the course extensively): Post first message as a "publicly" chosen randomness for all to see and then apply Fiat-Shamir heuristics to get non-interactive proofs.
