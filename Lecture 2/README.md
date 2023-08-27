> Lecture 1 talked about non-interactive proofs but rest of the course is focused on non-interactive proofs.

### Arithmetic circuit

Fix a finite field F = {0, ..., p-1} for some prime > 2.

C: F^n --> F

Public arithmetic circuit: C(x, w) --> F
x : public statement in F^n
w: secret witness in F^n

|C| = # gates in C.

### NARK (Non-interactive Argument of Knowledge)

A preprocessing NARK is a triple (S, P, V):

- Preprocessing (setup): S\(C\) -> public parameters (pp, pv) for prover and verifier
- P(pp, x, w) -> proof _pi_
- V(vp, x, _pi_) -> accept or reject

**Requirements**

- Complete: Verifier always accepts a valid proof.
- Knowledge Soundness: If verifier accepts the prove from P, then P "knows" w s.t. C(x, w) = 0.
- Optionally:
  - Zero knowledge: (C, pp, vp, x, _pi_) "reveals nothing new" about w.

### Succinct NARK (SNARK)

A preprocessing NARK is a triple (S, P, V):

- Preprocessing (setup): S\(C\) -> public parameters (pp, pv) for prover and verifier
- P(pp, x, w) -> **short** proof _pi_; len(_pi_) = O(log|C|)
- V(vp, x, _pi_) -> **fast to verify**; time(V) = O(|x|, log|C|)

**Requirements**
Complete, Knowledge sound and optionally zero knowledge[ (C, pp, vp, x, *pi*) "reveals nothing new" about w].

#### Knowledge Soundness

![](https://hackmd.io/_uploads/B1FpHjps3.png)

> zk-SNARK: A SNARK that is "zero-knowledge".

### Types of preprocessing Setup

1. **Trusted setup per circuit**: S(C; r) -> public parameters (pp, pv). Random _r_ must be kept secret from prover.
2. **Trusted but universal (updatable) setup**: secret _r_ is independent of C.
   S = (S_init, S_index)
   S_init(lambda; r) -> gp (global parameters)
   s_index(gp, C) -> (pp, pv)
3. **Transparent setup**: S\(C\) does not use secret data

![](https://hackmd.io/_uploads/SkZ7ropi2.png)

> - Prover time is almost linear in |C| for all of them.
> - It is the PCS that determines whether a proving system would require a trusted setup or not.

## Building an efficient SNARK

![](https://hackmd.io/_uploads/r1lULXCin.png)

- Cryptographic object: Security depends on certain cryptographic assumpitons.
- Info theoretic object: Can prove security of an IOP unconditinoally without any assumptions.

#### Commitments

- commit(m, r) -> com
- verify(m, com, r) -> accept or reject

**Properties**

1. **Binding**: cannot produce _com_ and two valid openings for _com_.
2. **Hiding**: _com_ reveals nothing about the commited data

## Functional commitment scheme

![](https://hackmd.io/_uploads/SJsDdXAs3.png)

### Four types of functional commitments

![](https://hackmd.io/_uploads/B1Pxt70oh.png)

### Polynomial commitment scheme (PCS)

![](https://hackmd.io/_uploads/r1AA67Co2.png)

### Types of PCS

| Protocol     | Trusted Setup | Using                   | Verification | Proof size |
| ------------ | ------------- | ----------------------- | ------------ | ---------- |
| KZG'10       | Yes           | Bilinear Groups         | O(1)         | Short      |
| Dory'20      | No            | Bilinear Groups         | O(log n)     | Short      |
| FRI          | No            | Hash Functions          | O(log n)     | Long       |
| Bulletproofs | No            | Elliptic Curves         | O(d)         | Short      |
| Dark'20      | No            | Groups of Unknown Order | O(1)         | Medium     |

> Todo: Confirm this table.

**Fiat-Shamir Transform**: public-coin interactive protocol => non-interactive protocol

- Idea: prover generates verifier's random bits on its own using hash function H.

> A useful trick!
> Zero test: If a function is zero at a random point r, then it is identically zero everywhere.
> Equality test: If two functions are equal at a random point r, then they are equal everywhere.

## Interactive Oracle Proof

F-IOP: A proof system that proves C(x, w) = 0 as follows:

- Setup\(C\) -> public parameters pp and vp = (f*0, f*-1, ..., f\_-s) [oracles for functions in F]

![](https://hackmd.io/_uploads/Hypld40o2.png)

> Verifier is given oracle access to all the functions that were sent to the verifier.

_Properties_

- Complete
- Knowledge Sound
- Optional: Zero Knowledge

### Example IOP

![](https://hackmd.io/_uploads/Sk5T3EAj2.png)
Note: Since we are comparing them at random points. If they are equal at random point, then the polynomials are equal with a very high probability.

- In IOPs we only specify what oracles are sent to the verifier.
- And then where does the verifier queries this oracles.
- Verifier can query the polynomial oracles at any point of their choice.

  > When we instantiate it with an actual PCS, means that r would get sent to the prover. the prover evaluates the polynomial f, and returns the result along with the proof that the evaluation was done correctly.

- IOPs are instantiated by using PCS. It's often called a compilation step, where we compile a Polynomial-IOP to a SNARK, using a PCS.

### Types of IOP

![](https://hackmd.io/_uploads/HkcfR4Ri2.png)
