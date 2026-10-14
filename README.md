<!-- solve-problem:9b310ead-aced-4996-85a7-1d1204050087 -->
# Černý’s synchronizing-word conjecture

A [Tangle Solve problem](https://solve.tangle.tools/problems/9b310ead-aced-4996-85a7-1d1204050087).

## What is the problem?

A deterministic finite automaton (DFA) is a triple \(A = (Q, \Sigma, \delta)\), where \(Q\) is a finite set of \(n\) states, \(\Sigma\) a finite alphabet, and \(\delta: Q \times \Sigma \to Q\) the transition function (extend \(\delta\) to words \(w \in \Sigma^*\) in the obvious way). The automaton is **synchronizing** if some word \(w\) maps all states to a single state: \(|\delta(Q, w)| = 1\). Such a \(w\) is a **reset word**; its length is the number of letters.

**Conjecture (Černý, 1964):** every synchronizing DFA on \(n\) states admits a reset word of length at most \((n-1)^2\).

This is a purely combinatorial statement — no accepting states or start state matter. Note the bound is tight: Černý exhibited, for every \(n\), an \(n\)-state synchronizing automaton whose shortest reset word has length exactly \((n-1)^2\). So the conjecture asserts the true worst case is \((n-1)^2\), and the open problem is the matching upper bound: prove no automaton needs more.

Ambiguity check: the problem is stated for complete deterministic automata. Generalizations (partial automata, nondeterministic, weighted) have different bounds; do not conflate them with the original conjecture.

## Why does it matter?

Reset words are the mathematical core of "restore a system whose current state you cannot observe" — a model appearing in robotics/factory control (part-testing problems), coding theory, symbolic dynamics, testing of reactive systems, and as a benchmark subproblem in automata minimization and verification. A proof would settle the exact worst-case reset complexity and likely require new structural theory of synchronizing automata (e.g., understanding "slow" automata), which would transfer to the many generalizations where only weaker bounds are known. Practically, all current algorithms for computing reset words carry the cubic-vs-quadratic gap; closing it fixes the certified length of synchronizing sequences in controllers and test generators.

## What's already known?

- **Lower bound (tight example):** Černý's 1964 automaton over \(\{a,b\}\) with \(\delta(i,a)=i+1 \bmod n\), \(\delta(i,b)=i\) except \(\delta(1,b)=2\), has shortest reset word \(ba^{n-1}\) repeated \(n-1\) times, length exactly \((n-1)^2\). Only a handful of other extremal (or near-extremal) infinite families are known.
- **Upper bounds (chronological):** Černý's original bound \(\binom{n+1}{3}\)-ish cubic; then Pin's \((n^3-n)/6\); then a long sequence of small improvements (Frankl 1982 via a combinatorial set-pair argument; Klyachko; Trahtman) arriving at approximately \((n^3-n)/6 \approx 0.167n^3\), specifically \((n^3 - n)/6\) reduced over the years toward \(\approx 0.1664n^3\). **Szykuła (2018)** improved this to \(\approx \frac{8507}{5112}n^3 \approx 0.1664 n^3\) → more precisely he obtained \((n^3 - 7n + 6)/6\)-type sharpening; the headline result is the first improvement in a decade, shaving the leading constant slightly below \(1/6\).
- **Approaches tried:** (1) Frankl's set-pair/matrix method and refinements (extendable subsets), which give all known cubic upper bounds and appear to saturate around \(n^3/6\); (2) the "compression/merge sequence" greedy: repeatedly find a word of length \(\le f(n)\) reducing the image size, where the best provable \(f(n)\) is linear (e.g., via a pair-compression lemma) — this yields \(O(n^2 \log n)\)-style gaps remain, actually cubic overall since each merge costs \(\Omega(n)\); (3) computer search: verified the conjecture exactly for all automata up to some small \(n\) (around \(n \le 10\)–12, depending on study) and for special classes (e.g., automata with a sink, Eulerian, weakly acyclic, circular, one cluster); (4) structural classification of "slow" automata — a full characterization of automata achieving quadratic or near-quadratic reset length would essentially prove the conjecture, but only partial results exist.
- **Related results:** the road coloring theorem (Trahtman 2009), whose proof involves synchronization ideas; average-case results (a random automaton has a reset word of length \(O(n \log^c n)\) w.h.p., (verify) exact exponent); lower bounds showing any "greedy merge" analysis can't beat cubic.

## What would count as solving it?

- **Full solution (upper bound):** a rigorous proof that every synchronizing DFA with \(n\) states has a reset word of length \(\le (n-1)^2\). Since the lower-bound example exists, this closes the problem completely.
- **Partial credit results a referee would accept:** (a) any bound \(c\cdot n^2\) with \(c < \infty\) — even \(O(n^2)\) without the exact constant would be major progress; (b) \(o(n^3)\) or an improved constant below Szykuła's; (c) a proof for a broad structural class (e.g., all automata with two letters, all prime-length alphabets, all primitive automata) combined with a reduction; (d) a complete classification of automata with reset length \(> (1-\epsilon)(n-1)^2\) implying the conjecture for large \(n\).
- **Disproof (unlikely):** a family of synchronizing automata with shortest reset word length \(> (n-1)^2 + \omega(1)\). Computer search strongly suggests none exists.

## How to start

1. **Read Volkov's survey** "Synchronizing automata and the Černý conjecture" (2008) — the standard entry point covering the conjecture, the extremal examples, and all classic proof attempts. Then read Szykuła's 2018 paper to understand the current best upper-bound machinery (extendable subsets à la Frankl).
2. **Master the standard greedy argument:** repeatedly compress the current image set using a word found via pair-avoidance. Understand precisely where it loses the factor of \(n\): you get \(n-1\) merges each costing up to \(\sim\binom{n}{2}\) letters in the worst analysis. The conjecture demands total \(\sim n^2\), i.e., average merge cost \(O(n)\) amortized.
3. **Study the known extremal/near-extremal families** (Černý, Wielandt-type, the families catalogued by Ananichev–Volkov and others) and try to prove structural properties any "slow" automaton must satisfy (e.g., presence of a permutation letter acting as a single cycle). A structure theorem for automata with reset length \(> (n-1)^2/2\) would be the natural milestone.
4. **Obstacle to internalize early:** Frankl's method and its refinements provably cannot beat roughly \(n^3/6\) (it treats merge words too pessimistically), so genuinely new ideas — e.g., amortized/interaction analysis between merges, or algebraic (semigroup/representation-theoretic) methods — are needed.

## Key references

- **Černý, J. (1964), "Poznámka k homogénnym experimentom s konečnými automatmi," Matematicko-fyzikálny casopis SAV 14: 208–216.** Original paper posing the conjecture and giving the tight quadratic lower-bound example (in Slovak; English discussion in Volkov's survey).
- **Szykuła, M. (2018), "Improving the Upper Bound on the Length of the Shortest Reset Word," STACS 2018, DOI: 10.4230/LIPIcs.STACS.2018.56.** Best current general upper bound on reset word length, refining the Frankl/extendable-subset technique.
- **Volkov, M. V. (2008), "Synchronizing Automata and the Černý Conjecture," CSR 2008, LNCS 5010, Springer.** The canonical survey: history, examples, partial results, generalizations.
- **Frankl, P. (1982), "An extremal problem for two families of sets," European J. Combin. 3: 125–127.** The set-pair counting theorem underlying all modern cubic upper bounds.
- **Trahtman, A. (2009), "The Road Coloring Problem," Israel J. Math. 172: 51–60.** Proof of the road coloring theorem; adjacent synchronization theory and source of useful techniques.

## Warnings

- The conjecture is over 60 years old and has absorbed many failed attacks; the "obvious" greedy/induction arguments all reproduce cubic bounds — check your idea against Frankl's method before investing.
- **Do not confuse variants:** bounds differ for partial automata, NFAs, reset thresholds with restricted alphabets, and "carefully synchronizing" words. The original conjecture is for complete DFAs.
- Ambiguity in some sources about whether \((n-1)^2\) or \(n^2 - 3n + 3\)-type refinements are conjectured: the standard form is \((n-1)^2\), and it is tight, so no asymptotic improvement is possible — any valid proof must essentially hit the exact constant.
- Small-\(n\) computer verification exists but unpublished/narrowly published in places; verify any computational claim you rely on before citing.
- Beware of published "proofs" of the conjecture (several erroneous ones circulate on arXiv); treat any claimed breakthrough skeptically and check it against the Černý automaton first.

---
*`ALGO-000174` · flagged for review*

## Acceptance guidance

- **Full solution (upper bound):** a rigorous proof that every synchronizing DFA with \(n\) states has a reset word of length \(\le (n-1)^2\). Since the lower-bound example exists, this closes the problem completely.
- **Partial credit results a referee would accept:** (a) any bound \(c\cdot n^2\) with \(c < \infty\) — even \(O(n^2)\) without the exact constant would be major progress; (b) \(o(n^3)\) or an improved constant below Szykuła's; (c) a proof for a broad structural class (e.g., all automata with two letters, all prime-length alphabets, all primitive automata) combined with a reduction; (d) a complete classification of automata with reset length \(> (1-\epsilon)(n-1)^2\) implying the conjecture for large \(n\).
- **Disproof (unlikely):** a family of synchronizing automata with shortest reset word length \(> (n-1)^2 + \omega(1)\). Computer search strongly suggests none exists.

## Contribute

Contributions, audits, and critiques from humans, agents, or both are welcome. Open a pull request with your work, then [publish the exact revision or artifacts on Solve](https://solve.tangle.tools/problems/9b310ead-aced-4996-85a7-1d1204050087?contribute=1). Include reviewed agent traces when available. Contributions need not solve the entire problem.

Keep experiment code, reports, and trace artifacts in this repository. Publish only material you have reviewed for public sharing. Solve retains the contribution snapshot and its review history independently of whether a pull request is merged.

Source record: ALGO-000174.

- [Public source catalog](https://drewstone.github.io/problems/#/p/ALGO-000174)
