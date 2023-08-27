# Interactive Proofs (IP)

Lecture:[ ZKP MOOC Lecture 4: Interactive Proofs(IP)](https://youtu.be/4018OYyoAf8)
Author of notes: [0xSachinK](https://twitter.com/0xSachinK)

## Interactive Proofs

![](https://hackmd.io/_uploads/HyZtpMs2h.png)

- SNARKs are arguments and not Proofs.
- Proof vs Argument
  - Interactive proofs
    - Soundness guarantee will hold no matter how hard the prover works.
    - IPs aren't based on cryptographic assumptions on their soundness. They are sound against computationally unbounded provers.
  - Argument
    - Soundness only holds against polynomial-time provers, then the protocol is called an **interactive arugment**.
    - If the cheating prover is able to break some crypto function (say solve the DLOG problem in a gropu where it's thought to be unsolvable), then the cheating prover can produce a proof even when he doesn't know the witness.
- Soundness vs Knowledge Soundness
  - Soundness: V accepts => There exists w s.t. C(x, w) = 0.
  - Knowledge Soundness: V accepts => P "knows" w s.t. C(x, w) = 0.
    - Knowledge soundness is stronger.
- SNARGs vs SNARKs
  - SNARGs (succinct non-interactive argument)
    - Don't satisfy knowledge soundness. Only soundness.
  - SNARK (succinct non-interactive argument of knowledge)
    - Satisfy knowledge soundness. Hence a K at the end.

| Feature/Property    | Interactive Proofs (IP)                                                                                               | SNARKs                |
| ------------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------- |
| Interactivity       | Interactive                                                                                                           | Non-interactive       |
| Security Type       | Information Theoretically Secure (Statistically Secure)                                                               | Arguments (see above) |
| Knowledge Soundness | Not necessarily. Mostly just sound. (Often applied in settings where knowledge soundeness is not a meaningful notion) | Knowledge sound       |

> - Non-interactive _ness_ is important in the blockchain context. Cause interactive protocols only work for individual verifiers. And individually convincing each node in the network is infeasible.

## SNARKs from IP: Outline

![](https://hackmd.io/_uploads/H1Vvpzi3h.png)

Types of functional commitments

- Polynomial commitments
- Multilinear commitments
- Vector commitments

**Merkle trees for vector commitments**

![](https://hackmd.io/_uploads/BJzEJgFh3.png)

- Although doesn't work in practice, because number of leaves for a function would be of size of the order of the group |F|.
- Also the leaves could be evaluations of any function and not necessarily the function that the prover should commit to.

## IP design: Technical primitives

Generally speakig, IP tend to use mutlivariate polynomials, because roughly the reason, is that by using many variables (30 to 60), we are able to keep the total degree of the polynomial that arise within protocol, we are able to keep the proof are short and fast to verify.

### SZDL Lemma

![](https://hackmd.io/_uploads/r1tZgsjhh.png)

### Low-degree and multilinear extensions

![](https://hackmd.io/_uploads/Syq8fltn2.png)

![](https://hackmd.io/_uploads/SyLw-si2n.png)

- Multilinear extensions are helpful in PCS, because if two functions differ even on a single point, then their multilinear extensions differ on almost all points. This helps with checking the diff between two functions.
- The ability of a total degree of a multivariate polynomial to be much lower than the number of coefficients, is exactly why multivariate polynomials are so useful in interactive proofs.

#### Langrange interpolation

[x]

- Important:
  - If we feed an algorithm a description of a function f whose domain is {0, 1}^l (consists of all 2^l evaulations of f), then it is possible to evaluate all the multi-linear extension of f, at any desired point, in O(2^l) time, which is only constant time slower than what's simply to read the description of f.

## The Sum-check protocol

![](https://hackmd.io/_uploads/rkHn-Ly63.png)

![](https://hackmd.io/_uploads/Syn3bUyah.png)

### Costs of the sum-check protocol

![](https://hackmd.io/_uploads/Sk0eM8yT3.png)

## Polynomial IOP underlying the SNARK

1. Given an (log S)-variate polynomial h, identify a related (3logS)-variate polynomial g_h such that:
   - h extends a correct transcript T <--> g_h(a, b, c) = 0 for all (a, b, c) belongs to {0, 1} ^ 3logS
2. Design an interactive proof to check that g_h(a, b, c) = 0 for all (a, b, c) belongs to {0, 1}^3logS.
   - In which V only needs to evaluate g_h\(r\) at one point r.

### Step 1

![](https://hackmd.io/_uploads/r1l1Q81p3.png)

## Step 2

![](https://hackmd.io/_uploads/H1w1QL1ph.png)
