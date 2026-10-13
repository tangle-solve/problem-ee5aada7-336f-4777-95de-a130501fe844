<!-- solve-problem:ee5aada7-336f-4777-95de-a130501fe844 -->
# Sliding Scale Conjecture

A [Tangle Solve problem](https://solve.tangle.tools/problems/ee5aada7-336f-4777-95de-a130501fe844).

## What is the problem?

The **Sliding Scale Conjecture** (Bellare, Goldwasser, Lund, Russell, 1993) concerns the achievable tradeoff in Probabilistically Checkable Proofs (PCPs) between three parameters:

- **Soundness** $s$: the probability the verifier accepts a false statement.
- **Alphabet size** $\Sigma$: the number of possible answers to each verifier query (equivalently, the proof's "alphabet").
- **Randomness** $r$ (equivalently proof length $2^r$) and query complexity.

**The conjecture (standard formalization):** For every soundness $s > 0$, there exist PCPs with soundness $s$, constant query complexity, alphabet size $\mathrm{poly}(1/s)$, and randomness $r = O(\log n + \log(1/s))$ — i.e., proof length polynomial in $n$ and $1/s$. The key point is that as $s$ slides down, the alphabet grows only polynomially in $1/s$ while the proof length grows only polynomially, and the number of queries stays constant. The conjecture is often stated for a binary alphabet, where one instead allows answer length $O(\log(1/s))$ bits per query and query complexity $O(1)$.

The trivial obstruction: with $q$ queries over alphabet $\Sigma$, soundness is at least roughly $1/\Sigma^q$ by a random-wrong-proof argument, so $\Sigma \geq (1/s)^{\Omega(1/q)}$ is necessary. The conjecture asserts this is essentially achievable.

## Why does it matter?

A resolution would yield tight inapproximability for many optimization problems (e.g., MAX-3SAT, labeling problems, min-coloring-type problems) with hardness thresholds scaling as $1 - \Theta(s)$ while reductions blow up instance size by only $\mathrm{poly}(1/s)$ — currently, such hardness factors come with exponential dependence on $1/s$ in reduction size, making the results qualitatively weaker. The conjecture is also equivalent to the existence of **locally testable codes** with tiny error probability, small alphabet, and near-linear length, so it directly targets a coding-theoretic gap. It is a benchmark problem for PCP construction technology (composition, degree reduction, aggregation).

## What's already known?

- **Original formulation:** Bellare–Goldwasser–Lund–Russell (1993, "Efficient probabilistically checkable proofs and an application to approximation") stated the conjecture and gave partial tradeoffs.
- **Constant soundness regime:** The PCP theorem gives $s$ a fixed constant with constant alphabet and logarithmic randomness — solved by dinur-style gap amplification and the original ALMSS/AS constructions.
- **Low-soundness, large alphabet:** Raz's parallel repetition and related "projection PCPs" (Raz, Moshkovitz–Raz, Dinur–Harsha–Kindler–Khot–Regev–Steurer) achieve soundness $s$ with alphabet $\exp(\mathrm{poly}(1/s))$ — super-polynomial, hence short of the conjecture. The DHKRS line ("short PCPs with polylog(n) query complexity") gives tradeoffs with query or alphabet penalties.
- **Moshkovitz (2019, "The Sliding Scale Conjecture")**: surveys the landscape, isolates the precise missing ingredient, and highlights what a construction would need; to my knowledge it does not resolve the conjecture (verify the paper's exact contribution).
- **Coding-theoretic view:** the conjecture amounts to a length–distance–locality tradeoff for locally testable codes that current constructions (e.g., Dinur–Evra–Livne–Lubotzky–Mozes high-dimensional expanders, giving constant-rate LTCs) do not simultaneously meet.

No construction is known achieving alphabet $\mathrm{poly}(1/s)$ and constant queries at soundness $s$ with $\mathrm{poly}(n, 1/s)$ proof length.

## What would count as solving it?

1. **(Positive construction — the main goal):** An explicit PCP verifier that, for every $s \geq 1/\mathrm{poly}(n)$, uses $O(1)$ queries, alphabet $\Sigma = \mathrm{poly}(1/s)$ (or binary answers of length $O(\log 1/s)$), and randomness $O(\log n + \log(1/s))$, with completeness 1 and soundness $s$. A referee needs the full correctness proof, including the composition/decoding steps.
2. **(Lower bound / impossibility):** A proof that some regime of the tradeoff is unattainable, e.g., that soundness $s$ with constant queries requires alphabet or proof length exceeding the conjectured bound.
3. **(Partial regimes):** Any construction matching the conjecture for a restricted range (e.g., $s \geq n^{-\epsilon}$ for all $\epsilon < 1$) with tight parameters would be major progress.

## How to start

1. **Read Moshkovitz (2019) first** — it is the modern statement of the problem and pinpoints where current techniques fail. Then read the original BGLR 1993 paper for the base formulation and the connection to approximation hardness.
2. **Understand the coding-theoretic equivalence:** a PCP with the sliding-scale parameters corresponds to a locally testable code with small error; study the DHKRS "projection PCP / agreement testing" framework, which is the closest existing technology — the obstacle there is the $\exp(1/s)$ alphabet from parallel-repetition-style amplification.
3. **Examine recent high-dimensional-expander LTCs (Dinur–Evra–Livne–Lubotzky–Mozes, 2022)**: these achieve constant rate and constant locality; check whether their testers can be amplified to soundness $1/\mathrm{poly}$ without alphabet blowup via aggregation over HDXs.
4. **Main obstacle:** gap amplification techniques (repetition, degree reduction) either blow up the alphabet exponentially in $1/s$ or blow up proof length/queries; the challenge is an amplification that degrades the tradeoff only polynomially.

## Key references

- Bellare, Goldwasser, Lund, Russell. *"Efficient Probabilistically Checkable Proofs and an Application to Approximation."* STOC 1993. Original formulation of the sliding-scale conjecture and the connection to approximation hardness with polynomial-sized reductions.
- Moshkovitz, Dana. *"The Sliding Scale Conjecture."* (2019, survey/reformulation). Modern statement of the conjecture, the known parameter regimes, and identification of the missing construction technique (verify publication venue).
- Dinur, Harsha, Kindler, Khot, Regev, Steurer. *"Short PCPs with polylog(n) query complexity."* STOC 2013 (approximate title — verify). Achieves low soundness with polylog query complexity, illustrating the query-vs-alphabet tradeoff frontier.
- Raz, Ran. *"A Parallel Repetition Theorem."* SIAM J. Computing, 1998. The tool behind the best low-soundness amplification, and the source of the $\exp(1/s)$ alphabet blowup that must be circumvented.

## Warnings

- **Parameter counting traps:** the conjecture demands *all three* parameters simultaneously; constructions that fix soundness while blowing up proof length exponentially in $1/s$ do not count.
- **Binary vs. large alphabet formulations differ**; be explicit which variant you target — converting between them costs query complexity.
- The census record is vague; the precise "best known" bounds should be verified against Moshkovitz (2019) and recent ECCC postings, as partial results may have appeared since (verify anything post-2019).

---
*`ALGO-000103` · unverified*

## Acceptance guidance

1. **(Positive construction — the main goal):** An explicit PCP verifier that, for every $s \geq 1/\mathrm{poly}(n)$, uses $O(1)$ queries, alphabet $\Sigma = \mathrm{poly}(1/s)$ (or binary answers of length $O(\log 1/s)$), and randomness $O(\log n + \log(1/s))$, with completeness 1 and soundness $s$. A referee needs the full correctness proof, including the composition/decoding steps.
2. **(Lower bound / impossibility):** A proof that some regime of the tradeoff is unattainable, e.g., that soundness $s$ with constant queries requires alphabet or proof length exceeding the conjectured bound.
3. **(Partial regimes):** Any construction matching the conjecture for a restricted range (e.g., $s \geq n^{-\epsilon}$ for all $\epsilon < 1$) with tight parameters would be major progress.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/ee5aada7-336f-4777-95de-a130501fe844?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000103.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000103)
