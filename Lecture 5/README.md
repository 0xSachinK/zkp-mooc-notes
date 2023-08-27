# Plonk Interactive Oracle Proofs (IOP)

Lecture:[ ZKP MOOC Lecture 5: Plonk Interactive Oracle Proofs (IOP)](https://youtu.be/A0oZVEXav24) | Author of notes: [0xSachinK](https://twitter.com/0xSachinK)

## KZG PCS

This lecture gives an introductory explanation. I skipped making notes of that as I have already made detailed [notes of lecture 6](https://hackmd.io/S9L9JGWUQ2W-2-NA24-5KQ) that dives alot deeper into KZG.

### Properties of KZG

**Generalizations**

- Can also use KZG to commit to k-variate polynomials

**Batch Proofs**

- Suppose verifier has commitments com_f1, com_f2, ... com_fn
- Prover wants to prove f_i(u_i,j) = v_i,j for i belongs to n, j belongs to m
- batch proof _pi_ is only one group element

**Linear time commitment**

- Two ways to represent a poly
  - Coefficient representation
    - f(X) = f0 + f1X + ... + fd(X^d)
  - Point-value representation
    - (a0, f(a0)), (a1, f(a1)), ..., (ad, f(ad))
- We have point-value representation. To commit to it in linear time
  - Naive way:
    - Going from point-value representation to coeff representation
      - takes time O(dlogd) using Number Theory Transform (NTT)
  - Better way:
    - ![](https://hackmd.io/_uploads/HJV-rzDa2.png)
      - Compute lagrange interpolation
      - Lagrange interpolation only depends on the values at which we evaulate the polynomial (the x coordinates, the a_i and a_j's)
      - Commitment involves sum of dot products between the polynomial evaluations (f0(a0), ... fd(ad)) and the global parameters
      - Linear time.

> Note: Usually the global parameters are in the lagrange basis.

**Multiple evaluation proofs**

- Can generate many evaluation proofs at once relatively quickly
  - For d points, naive takes time O(d^2)
  - FK algorithm:
    - If the points at which we are evaluating the poly are part of a multiplicative subgroup then it takes time O(dlogd)
    - Otherwise using FK algorithm takes O(d \* log^2 d), still better than O(d^2)

**Difficulties with KZG** - Trusted setup for gp, and gp size is linear in d (thus quite large)

## Proof gadgets

**Recall polynomial testing**

- For two bounded degree polynomial (degree <= d), if f\(r\) = g\(r\), for a random r in Fp, then f = g.

**Polynomial equality testing with KZG**

- Say, if prover commits to 4 polynomials, com(f), com(g1), com(g2), comg(g3).
- Then to test if f = g1 _ g2 _ g3, Verifier can't just compare the commitments. Hence it queries all four poly at random point r and tests equality

**Important proof gadgets for univariates**

- Let Omega be some substet of Fp of size k
- ZeroTest
  - Prove that f is identically zero on Omega
- SumCheck
  - Prove that the sum of all evaluations of f over omega is 0
- ProdCheck
  - Prove that the product of all evaluations of f over omega is 1

**Vanishing polynomial**

- Is zero on all of set Omega
- Z(X) = (X - a1)(X - a2)...(X - ak) for all a1, a2, .. ak belongs to omega
- Degree of Z(X) is k
- If omega is set of all roots of unity, then vanishing polynomial would be X^k - 1
- And it can be computed in logarithmic time O(log k)

### Zero test

- On set _Omega_
- Lemma: f is zero on omega if and only if f(X) is divisible by Z(X)
- Prover:
  - Evaluates q(X) <-- f(X) / Z(X)
  - Commits to q(X)
  - Time: Dominated by time to compute q(X) and then commit to q(X)
- Verifier:
  - Queries q(X) and f(X) at r
  - Learns q\(r\), f\(r\)
  - Accept if f\(r\) = q\(r\) \* Z\(r\)
  - Time: O(log k) to compute Z\(r\) and two poly queries

### Product check

- On set _Omega_
- Let t be a poly such that
  - t(1) = f(1)
  - t(w) = f(1) \* f(w)
  - t(w^2) = f(1) _ f(w) _ f(w^2)
  - ...
  - t(w^k-1) = f(1) _ f(w) _ f(w^2) _ ... _ f(w^k-1)
- t(w _ x) = t(x) _ f(w \* x) for all x in omega
- Lemma:
  - If
    - t(w ^ k-1) = 1 and
    - t(w _ x) - t(x) _ f(w \* x) = 0 for all x in omega
  - then product of all evaluations of f over omega is 1
- Prover
  - t(w _ x) - t(x) _ f(w \* x) = 0 for all x in omega [This is essentially a zero test]
  - Evaluates q(X) <-- t1(X) / X^k - 1
    - Where t1(X) = t(w _ x) - t(x) _ f(w \* x)
  - Commits to q(X) and t1(X)
  - Proof size: Two commits, 1 batch evaluation
  - Prover time dominated by time to compute the quotient polynomial, O(klogk)
- Verifier
  - Queries t(X) at w^k-1, r, wr
  - Queries q(X) at r and f(X) at wr
  - Learn t(w^k-1), t\(r\), t(wr), q\(r\), f(wr)
  - Accept if t(w^k-1) = 1
  - Accept if t(wr) - t\(r\)_ f(wr) = q\(r\) _ (r^k - 1)
  - Time: O(log k) to compute (r^k - 1) and w^k-1 and two poly queries

> Sum check is basically the same as product check.

### Permutation check

- Prover wants to prove that two tuples are permuations of one another
  - Tuple 1: (f(1), f(w), ..., f(w^k-1))
  - Tuple 2: (g(1), g(w), ..., g(w^k-1))
- f*hat(X) = (X - f(1)) * (X - f(w)) \_ ... \* (X - f(w^k-1))
- g*hat(X) = (X - g(1)) * (X - g(w)) \_ ... \* (X - g(w^k-1))
- To prove f_hat(X) = g_hat(X)
- Evaluate them at random point r, and prove f_hat(r) = g_hat(r)
  - Product check: f_hat\(r\) / g_hat\(r\) = 1

### Prescribed permutation check

- Prover wants to prove that two tuples are a specific permuation of one another
- Complex checks but can be achieved in linear time. Leverages product check.

## PLONK IOP

> Prover wants to prove C(x, w) = 0.

#### Step 0 (Witness Generations)

Compile circuit to a computation trace.

#### Step 1 (Arithmetization)

Encode the computation trace as a table. Then encode the trace as a polynomial T.
![](https://hackmd.io/_uploads/HyNtIQPTh.png)

### Step 2 (Proving validity of T)

- Prover needs to prove that T is a correct computation trace:
  - T encodes the correct inputs
  - Every gate is evaluated correctly
  - The wiring is implemented correctly
  - The output of last gate is 0
    - Easy: prove T(w_3|c| - 1) = 0 by opening it at that point. Basically the output of last gate is zero.

#### Proving 1: T encodeds the correct inputs

- Both prover and verifier interpolate a polynomial v(X) that encodes the x-inputs to the circuit: v(w ^ -j) = input #j
- Prover proves (1) by using a ZeroTest on set of omega_input to prove that
  - T(y) - v(y) = 0 for all y in omega_input

#### Proving 2: Every gate is evaluated correctly

![](https://hackmd.io/_uploads/SyXbOXPT2.png)

- Encodes both addiiton and multiplication gates into one polynomial. Thus proving that all gates were evaluated correctly.
- Prover users ZeroTest to prove that for all y in omega_gates:
  - S(y) * [T(y) + T(w*y)] + (1 - S(y)) _ T(y) _ T(w _ y) - T(w^2 _ y) = 0

#### Proving 3: The wiring is correct

To check that T is an invariant under the permutation W, we use the prescribed permutaion check.

![](https://hackmd.io/_uploads/H106YmPan.png)

> W doesn't depends on the inputs. It is an intrinsic property of the circuit itself. Generated during the setup algorithm.

![](https://hackmd.io/_uploads/r1eysQP6h.png)

> This convinces the verifer T is a commitment to a valid computation trace. And the output of this computation is 0.

The Plonk Poly-IOP is complete and knowledge sound

- Assuming 7|C|/p is negligible (almost always cause p is large.)
- We trust the paper!

**PLONK is a SNARK**

- The proof is short (O(1) commitments).
- The verifier is logarithmic, hence fast.
- The SNARK can easily be made into a zk-SNARK.
- Main challenge: Reduce prover time. Run time is quasi-linear, O(|C| log |C|).

**HyperPlonk**

- Replaces set omeaga with {0, 1}^t
- The polynomial T is now a multilinear polynomial in t variables
- ZeroTest is replaced by a multilinear SumCheck (linear time)
- Hence we end up with a linear time prover

### A generalization: plonkish arithmetization

- We replace simple gates, left + right = output, and left \* right = output
- With more generic gates.
- This custom gate is verified on the entire trace (on each and every row of the matrix).
  - We multiply custom gates with selector polynomial to determine on which rows we are going do that check, aka activating the gate.

![](https://hackmd.io/_uploads/SyuKz4Da3.png)

### Plonk IOP with other PCS

The PLONK IOP can be combined with other commitment scheme to create other SNARK systems.
![](https://hackmd.io/_uploads/B1FWrXwpn.png)
