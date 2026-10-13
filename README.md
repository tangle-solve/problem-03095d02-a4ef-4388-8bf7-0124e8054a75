<!-- solve-problem:03095d02-a4ef-4388-8bf7-0124e8054a75 -->
# Classical polynomial-time integer factorization

A [Tangle Solve problem](https://solve.tangle.tools/problems/03095d02-a4ef-4388-8bf7-0124e8054a75).

## What is the problem?

Does there exist a (deterministic or randomized) classical Turing-machine algorithm that, given an integer N with bit length n = ⌈log₂ N⌉, outputs the complete prime factorization of N in time polynomial in n?

**Formal setting:** Input N ∈ ℤ, N ≥ 2, written in binary. Output the multiset of primes p₁,…,pₖ with ∏pᵢ = N. Runtime must be poly(n) = n^O(1). Randomized variants: Las Vegas (always correct, expected poly time) or Monte Carlo (bounded error). The problem is in NP ∩ co-NP; it is not known to be NP-complete, and is widely believed to be easier than NP-complete problems — the barrier is that no polynomial-time algorithm is known despite intense effort since Gauss.

Note the distinction between *finding* the factorization and *deciding* primality: primality testing is in P (AKS, 2002), but extracting factors is the open problem. Also note "polynomial in bit length," not in N itself — trial division is already O(√N).

**Ambiguity:** The census record says "deterministic or randomized"; these are separate open questions. A Las Vegas algorithm is the strongest natural target; heuristically-analyzed algorithms without proven runtime (e.g., ECM) do not count.

## Why does it matter?

A polynomial-time classical factoring algorithm would break RSA and factoring-based cryptography wholesale (though practitioners would migrate to post-quantum schemes already standardized by NIST, since Shor's algorithm already forces that migration). Theoretically, it would resolve a decades-old question in computational number theory, likely introduce powerful new algebraic or analytic machinery, and inform related problems in algorithmic number theory (discrete log, class-group computation). Conversely, persistent failure motivates lattice-based and isogeny-based cryptography assumptions — though factoring being hard is not provably necessary for any of these.

## What's already known?

**Best proven general classical bounds:** The Number Field Sieve (NFS) runs in heuristic expected time exp((c+o(1)))(ln N)^(1/3)(ln ln N)^(2/3)) with c = (64/9)^(1/3) ≈ 1.923 for the general case. This is subexponential in bit length n, since the exponent grows as n^(1/3)(log n)^(2/3). Lenstra & Lenstra (1993) documents its development. Proven (non-heuristic) runtimes are weaker; the best rigorous bounds come from variants like the NFS with provable parameters and from Pollard/Strassen-type methods, still subexponential.

**Special-purpose algorithms:** ECM (Lenstra 1987) runs in subexponential time in the size of the smallest factor — polynomial when the second-largest prime factor is small. SQUFOF, Pollard rho, p−1, p+1 are slower but simple.

**What's been tried and why it falls short:**
- Smoothness-based sieves (quadratic sieve, NFS): rely on finding smooth numbers; smoothness density is inherently subexponential.
- Continued fractions / class groups / index calculus in various groups: same smoothness bottleneck.
- Lattice reduction (LLL): polynomial time but only yields relation-finding machinery; Lenstra's elliptic-curve and knapsack applications don't reach general factoring.
- Analytic approaches (e.g., trying to exploit distribution of primes): no reduction of factoring to any proven density statement is known.

**Quantum:** Shor (1997) gives polynomial time on a quantum computer — this does not answer the classical question but shows the problem is not information-theoretically hard.

**Complexity status:** Factoring is in BQP; not known in P; not known NP-hard (and believed not to be, since it lies in NP ∩ co-NP).

## What would count as solving it?

1. **Positive (algorithm):** An explicit classical algorithm with a *proven* worst-case (or expected, for Las Vegas) runtime of n^O(1) for arbitrary N, together with correctness proof. A Monte Carlo algorithm with error < 1/3 would also count, but Las Vegas is preferred since factoring can be verified.
2. **Partial positive:** A deterministic poly-time algorithm assuming standard but unproven hypotheses (GRH, Cramér-type smoothness) would be a major advance but not a full solution.
3. **Negative/separation:** A proof that no classical polynomial-time factoring algorithm exists — this appears far beyond current technique (would need at least P ≠ NP ∩ co-NP-type machinery for a structured problem; no plausible route exists).

A referee would reject: heuristic runtime analyses without proof, algorithms polynomial only for restricted integer families, or quantum algorithms (already done by Shor).

## How to start

1. **Read** Lenstra & Lenstra (1993) to understand NFS in depth, especially the asymptotic analysis and why the (ln N)^(1/3)(ln ln N)^(2/3) barrier arises from smoothness densities. Then Lenstra (1987) on ECM (Annals of Math) to see how the size of the smallest factor enters.
2. **Identify the core bottleneck:** every known general method reduces factoring to finding "smooth" (fully small-factored) numbers or relations, and the density of y-smooth numbers near N is provably subexponential. A polynomial algorithm must avoid smoothness sampling as the bottleneck, or find a way to make the smoothness bound polynomial.
3. **Candidate directions:** (a) reduce factoring to a problem with algebraic structure admitting polynomial algorithms (lattices, tensor ranks, p-adic lifting à la Hensel/coppersmith-type methods — see Coppersmith's small-root theorems as the closest known "poly-time partial factoring"); (b) exploit multiplicative structure via group theory beyond ℤ/nℤ* (ECM generalizes this line); (c) study why relativizing/algebraizing barriers don't obviously apply — factoring is algebraic, so natural proofs–style barriers are weaker here.
4. **Main obstacle:** decades of sharpening have only improved constants in the NFS exponent; there is no known conceptual route from subexponential to polynomial, and the smoothness-density obstruction looks fundamental to the relation-finding paradigm.

## Key references

- H. W. Lenstra, Jr. and C. Pomerance, "A Rigorous Time Bound for Factoring Integers" (JAMS 1992) — proves (non-heuristic) subexponential bounds via a variant of the quadratic/class group approach; the best *proven* general bound. (verify exact venue/year)
- A. K. Lenstra and H. W. Lenstra, Jr., eds., *The Development of the Number Field Sieve*, Lecture Notes in Mathematics 1554, Springer, 1993 — collects the papers establishing NFS, with the asymptotic heuristic analysis.
- P. W. Shor, "Polynomial-Time Algorithms for Prime Factorization and Discrete Logarithms on a Quantum Computer," SIAM J. Comput. 26(5), 1997; arXiv:quant-ph/9508027 — the quantum polynomial-time algorithm; shows the classical question is orthogonal to quantum solvability.
- H. W. Lenstra, Jr., "Factoring Integers with Elliptic Curves," Annals of Mathematics 126, 1987 — ECM; subexponential in the smallest factor, illustrating the special-case landscape.
- D. Coppersmith, "Small Solutions to Polynomial Equations, and Low Exponent RSA Vulnerabilities," J. Cryptology 10, 1997 — polynomial-time methods for finding factors given partial information; the closest existing "polynomial-time factoring fragment." (verify)
- M. Agrawal, N. Kayal, N. Saxena, "PRIMES is in P," Annals of Mathematics 160, 2004 — primality in P, clarifying what factoring must additionally do.

## Warnings

- **Do not count Shor's algorithm** or any quantum result; the question is strictly classical.
- **Heuristic analyses don't count.** NFS and ECM runtimes rely on smoothness assumptions that are empirically supported but unproven; a solution needs proven polynomial bounds.
- **Common dead ends:** trial-division-style improvements (only help with small factors); claiming NP-hardness implications (none known, and factoring's NP ∩ co-NP status argues against it); rediscovering Pollard rho/p−1 variants.
- **"Polynomial" means in bit length n**, not in N; check any claimed algorithm's complexity measure carefully.
- Partial-information factoring (Coppersmith) is not general factoring — it requires knowing bits of a factor in advance.

---
*`ALGO-000201` · unverified*

## Acceptance guidance

1. **Positive (algorithm):** An explicit classical algorithm with a *proven* worst-case (or expected, for Las Vegas) runtime of n^O(1) for arbitrary N, together with correctness proof. A Monte Carlo algorithm with error < 1/3 would also count, but Las Vegas is preferred since factoring can be verified.
2. **Partial positive:** A deterministic poly-time algorithm assuming standard but unproven hypotheses (GRH, Cramér-type smoothness) would be a major advance but not a full solution.
3. **Negative/separation:** A proof that no classical polynomial-time factoring algorithm exists — this appears far beyond current technique (would need at least P ≠ NP ∩ co-NP-type machinery for a structured problem; no plausible route exists).

A referee would reject: heuristic runtime analyses without proof, algorithms polynomial only for restricted integer families, or quantum algorithms (already done by Shor).

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/03095d02-a4ef-4388-8bf7-0124e8054a75?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000201.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000201)
