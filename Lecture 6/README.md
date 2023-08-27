# ZKP MOOC Lecture 6 (Notes)

**Generic construction of a polynomial commitment**

- keygen(lambda, F) -> global parameters (also called public key, or common reference string)
- commit(gp, f) -> com_f
- eval(gp, f, u) -> v, _pi_
- verify(gp, com_f, u, v, _pi_) -> accept or reject

## Background

### Group

![](https://hackmd.io/_uploads/SyigJF0s3.png)

> In cryptography, common gropus are positive integers mod P : {1, 2, ... p-1} under mult "x" operation or elliptic curves.

### Generator of a group

An element g that generates all elements in the group by taking all powers of g.

```
Examples: Z_7 = {1, 2, 3, 4, 5, 6}
Generator 3: 3^1 = 3; 3^2 = 2; 3^3 = 6; 3^4 = 4; 3^5 = 5; 3^6 = 1;
```

### Discrete Logarithm (DLOG) Assumption

A group G has an alternative representation as the powers of the generator g: {g^1, g^2, g^3, ... g^(p-1)}.

**Discrete Logarithm Problem**: given y ∈ G, find x s.t. g^x = y.

Assumption: Discrete-log problem is computationally hard.

> Note that: This doesn't hold in all groups. But is believed to hold in certain groups used in cryptography.

### Diffie-Hellman assumption

Computational DH assumption: Given G, g, g^x, g^y, cannot compute g^(xy)

### Billinear pairing

![](https://hackmd.io/_uploads/rJxrWY0j2.png)

- You can't break DH assumption using this.
- But pairing can be used to check that g^(xy) given g^x & g^y.
- Not all groups in which DLOG is hard are believed to support computing efficient pairings. But some groups defined by elliptic curve are.

**Example of billinear pairing: BLS Signature**

![](https://hackmd.io/_uploads/SJcJMK0s3.png)

## KZG polynomial commitment

**keygen(_lambda_, F) -> gp**

- Sample random _tau_ ∈ F
- gp = (g, g^tau, g^(tau^2), ..., g^(tau^d))
- **delete tau** !! (trusted setup)

**commit**

![](https://hackmd.io/_uploads/HJx_-EtAsn.png)

**eval**
![](https://hackmd.io/_uploads/rkhgHt0oh.png)

**verify**

![](https://hackmd.io/_uploads/S1cyLK0o3.png)

> Knowledge and Soundness hold. Trust me bro.

### Properties

- Keygen: Trusted setup ❗️❗️
- Commit: O(d) group exponentiations, O(1) commitment size
- Eval: O(d) group exponentiations
  - q(x) can be computed efficiently in linear time!
- Proof size: O(1), 1 group element
- Verifier time: O(1), 1 pairing

### Trusted setup ceremony

![](https://hackmd.io/_uploads/rkSUoYRs3.png)

## Variants of KZG Polynomial Commitment

### Multivariate poly-commit

![](https://hackmd.io/_uploads/SkJ1t9Cs3.png)

### Achieving Zero Knowledge

> Solution: Masking with randomizers. Introduces randomness. Not relevant right now.

### Batch opening: single polynomial

Prover wants to prove f at u1, u2, ...., um for m < d.

![](https://hackmd.io/_uploads/HyXj550sh.png)

### Batch opening: multiple polynomials

![](https://hackmd.io/_uploads/r1PRcc0ih.png)

## Polynomail commitments based on discrete-log

> All are without trusted setup.

- Bulletproofs
  - Keygen: O(d), Transparent setup!
  - Commit: O(d) group exponentiations, O(1) commitment size
  - Eval: O(d) group exponentiations
  - Proof size: O(log d)
  - Verifier time: O(d) ❗️❗️
- Hyrax
  - Verifier time: O(d^1/2)
  - Proof size: O(d^1/2)
- Dory
  - Improve verifier time to O(logd)
  - Key idea: Delegating the structured verifier computation to the prover using inner pairing product arguments
  - Also improves the prover time to O(d^1/2) exponentiations plus O(d) field operations
- Dark
  - Achieves O(log d) proof size and verifier time
  - Uses Group of unknown order

![](https://hackmd.io/_uploads/Hy2H1j0s2.png)
