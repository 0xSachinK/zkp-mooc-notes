# ZKP MOOC Notes

### Introduction

1. [Lecture 1: Introduction and History of ZKP](Lecture%201/README.md#introduction-and-history-of-zkp)

   - [Classical proofs](Lecture%201/README.md#classical-proofs)
   - [NP Language](Lecture%201/README.md#np-language)
   - [Zero Knowledge Interactive Proofs](Lecture%201/README.md#zero-knowledge-interactive-proofs)
     - [Interactive Proofs](Lecture%201/README.md#interactive-proofs)
     - [What is Zero Knowledge?](Lecture%201/README.md#what-is-zero-knowledge)
       - [Computational Indistinguishability](Lecture%201/README.md#computational-indistinguishability)
   - [Proof of Knowledge](Lecture%201/README.md#proof-of-knowledge)
   - [All of NP is Zero Knowledge](Lecture%201/README.md#all-of-np-is-zero-knowledge)
     - [3-colorable is ZKP](Lecture%201/README.md#3-colorable-is-zkp)
   - [Fiat-Shamir paradigm](Lecture%201/README.md#fiat-shamir-paradigm)

2. [Lecture 2: Overview of Modern SNARK Constructions](Lecture%202/README.md#overview-of-modern-snark-constructions)

   - [Arithmetic circuit](Lecture%202/README.md#arithmetic-circuit)
   - [NARK (Non-interactive Argument of Knowledge)](Lecture%202/README.md#nark-non-interactive-argument-of-knowledge)
   - [Succinct NARK (SNARK)](Lecture%202/README.md#succinct-nark-snark)
     - [Knowledge Soundness](Lecture%202/README.md#knowledge-soundness)
   - [Types of preprocessing Setup](Lecture%202/README.md#types-of-preprocessing-setup)
   - [Building an efficient SNARK](Lecture%202/README.md#building-an-efficient-snark)
   - [Commitments](Lecture%202/README.md#commitments)
   - [Functional commitment scheme](Lecture%202/README.md#functional-commitment-scheme)
   - [Four types of functional commitments](Lecture%202/README.md#four-types-of-functional-commitments)
   - [Polynomial commitment scheme (PCS)](Lecture%202/README.md#polynomial-commitment-scheme-pcs)
     - [Types of PCS](Lecture%202/README.md#types-of-pcs)
   - [Interactive Oracle Proof](Lecture%202/README.md#interactive-oracle-proof)
     - [Example IOP](Lecture%202/README.md#example-iop)
   - [Types of IOP](Lecture%202/README.md#types-of-iop)

3. Lecture 3: Libraries and Compilers to build ZKP

- You are better of reading their respective documents. Or watch these videos.

### Polynomial IOPs

4. [Lecture 4: Interactive Proofs (IP)](Lecture%204/README.md#interactive-proofs-ip)

   - [Interactive Proofs](Lecture%204/README.md#interactive-proofs)
   - [SNARKs from IP: Outline](Lecture%204/README.md#snarks-from-ip-outline)
   - [IP design: Technical primitives](Lecture%204/README.md#ip-design-technical-primitives)
     - [SZDL Lemma](Lecture%204/README.md#szdl-lemma)
     - [Low-degree and multilinear extensions](Lecture%204/README.md#low-degree-and-multilinear-extensions)
     - [Langrange interpolation](Lecture%204/README.md#langrange-interpolation)
   - [The Sum-check protocol](Lecture%204/README.md#the-sum-check-protocol)
     - [Costs of the sum-check protocol](Lecture%204/README.md#costs-of-the-sum-check-protocol)
   - [Polynomial IOP underlying the SNARK](Lecture%204/README.md#polynomial-iop-underlying-the-snark)
     - [Step 1](Lecture%204/README.md#step-1)
     - [Step 2](Lecture%204/README.md#step-2)

5. [Lecture 5: Plonk Interactive Oracle Proofs (IOP)](Lecture%205/README.md#plonk-interactive-oracle-proofs-iop)

   - [KZG PCS](Lecture%205/README.md#kzg-pcs)
     - [Properties of KZG](Lecture%205/README.md#properties-of-kzg)
   - [Proof gadgets](Lecture%205/README.md#proof-gadgets)
     - [Polynomial equality testing with KZG](Lecture%205/README.md#polynomial-equality-testing-with-kzg)
     - [Important proof gadgets for univariates](Lecture%205/README.md#important-proof-gadgets-for-univariates)
     - [Vanishing polynomial](Lecture%205/README.md#vanishing-polynomial)
     - [Zero test](Lecture%205/README.md#zero-test)
     - [Product check](Lecture%205/README.md#product-check)
     - [Permutation check](Lecture%205/README.md#permutation-check)
     - [Prescribed permutation check](Lecture%205/README.md#prescribed-permutation-check)
   - [PLONK IOP](Lecture%205/README.md#plonk-iop)
     - [Step 0 (Witness Generations)](Lecture%205/README.md#step-0-witness-generations)
     - [Step 1 (Arithmetization)](Lecture%205/README.md#step-1-arithmetization)
     - [Step 2 (Proving validity of T)](Lecture%205/README.md#step-2-proving-validity-of-t)
       - [Proving 1: T encodeds the correct inputs](Lecture%205/README.md#proving-1-t-encodeds-the-correct-inputs)
       - [Proving 2: Every gate is evaluated correctly](Lecture%205/README.md#proving-2-every-gate-is-evaluated-correctly)
       - [Proving 3: The wiring is correct](Lecture%205/README.md#proving-3-the-wiring-is-correct)
   - [PLONK is a SNARK](Lecture%205/README.md#plonk-is-a-snark)
   - [HyperPlonk](Lecture%205/README.md#hyperplonk)
   - [A generalization: plonkish arithmetization](Lecture%205/README.md#a-generalization-plonkish-arithmetization)
   - [Plonk IOP with other PCS](Lecture%205/README.md#plonk-iop-with-other-pcs)

### Polynomial Commitments

6. [Lecture 6: Discrete-log-based Polynomial Commitments](Lecture%206/README.md#discrete-log-based-polynomial-commitments)

   - [Generic construction of a polynomial commitment](Lecture%206/README.md#generic-construction-of-a-polynomial-commitment)
   - [Background](Lecture%206/README.md#background)
     - [Group](Lecture%206/README.md#group)
     - [Generator of a group](Lecture%206/README.md#generator-of-a-group)
     - [Discrete Logarithm (DLOG) Assumption](Lecture%206/README.md#discrete-logarithm-dlog-assumption)
     - [Diffie-Hellman assumption](Lecture%206/README.md#diffie-hellman-assumption)
     - [Billinear pairing](Lecture%206/README.md#billinear-pairing)
       - [Example of billinear pairing: BLS Signature](Lecture%206/README.md#example-of-billinear-pairing-bls-signature)
   - [KZG polynomial commitment](Lecture%206/README.md#kzg-polynomial-commitment)
     - [Properties](Lecture%206/README.md#properties)
     - [Trusted setup ceremony](Lecture%206/README.md#trusted-setup-ceremony)
   - [Variants of KZG Polynomial Commitment](Lecture%206/README.md#variants-of-kzg-polynomial-commitment)
     - [Multivariate poly-commit](Lecture%206/README.md#multivariate-poly-commit)
     - [Achieving Zero Knowledge](Lecture%206/README.md#achieving-zero-knowledge)
     - [Batch opening: single polynomial](Lecture%206/README.md#batch-opening-single-polynomial)
     - [Batch opening: multiple polynomials](Lecture%206/README.md#batch-opening-multiple-polynomials)
   - [Polynomial commitments based on discrete-log](Lecture%206/README.md#polynomial-commitments-based-on-discrete-log)

7. ZKP Based on Error-Correcting Codes [TBD]

8. Transparent ZKP [TBD]

### Linear PCP

8. Linear PCP [TBD]

### Recursive SNARKs

9. Recursive SNARKS [TBD]

### Topics
