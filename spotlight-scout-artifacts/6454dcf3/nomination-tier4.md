## Spotlight nomination — Tier 4

<a id="elevator"></a>
**Elevator pitch (≤25 words).** CER uses the LLM's own conditional generation probability as an implicit verifier, with a theoretical guarantee that expected CER equals exact-match reward — enabling gradient-friendly RLVR in any domain without a rule-based verifier.

<a id="surprise"></a>
**The load-bearing finding.** Theorem 2 (Section 2, Equations 14–15) proves that the expected CER loss Lρ(θ) = E_q∼D,(s,a)∼πθ(·|q)[ρ(a, a*(q))] = E_q∼D,(s,a)∼πθ(·|q)[I(a=a*(q))] — establishing CER as a formally grounded relaxation of binary exact-match rather than a heuristic soft reward, providing non-zero gradients where the original binary signal would be zero for all non-exact answers.

<a id="wave"></a>
**Research wave / era.** This paper extends the post-RLVR verifier-free research wave (RLPR, VeriFree, General-Verifier, Nover — all 2025). While concurrent work proposed model-intrinsic rewards as heuristics, CER provides the cleanest theoretical characterization: a formal equivalence theorem that distinguishes it from heuristic approaches and establishes precise conditions under which the heuristic succeeds.

<a id="counterfactual"></a>
**Counterfactual.** If this paper had been posted in 2022, the RLVR paradigm was not yet mainstream (DeepSeek-R1 and GRPO are 2025 events); the general-domain motivation would have been less urgent and the empirical focus on math benchmarks less compelling. In 2026, the research question is maximally relevant — RLVR is the dominant LLM reasoning paradigm and the field actively needs to remove the verifier bottleneck. CER provides the theoretical grounding that makes it a foundational building block rather than another heuristic trick.

<a id="audience"></a>
**Who must read this.** Required reading for: (1) **LLM reasoning researchers** — because CER provides the first theoretically principled verifier-free approach that extends RLVR to open-form answer domains where no rule-based verifier exists; (2) **RLVR practitioners** — because the Rule+CER combination (Table 1) shows the two approaches are complementary, not competitive, offering immediate practical utility; (3) **Reward design theorists** — because Theorem 2's equivalence result is a clean example of establishing formal guarantees for seemingly heuristic reward-shaping techniques.

<a id="prior-work"></a>
**Why this isn't derivable from prior work.** RLPR (Yu et al. 2025, arXiv:2506.18254) and VeriFree (Zhou et al. 2025) both extend RLVR to general domains using the model itself as a verifier, but treat the model-intrinsic reward as a heuristic approximation. CER's contribution is the identification that defining reward as ρ(a, a*) = E_{s'~πθ(·|q,a)}[πθ(a*|s', q)] — conditioning on the generated answer via the policy model — yields all four Properties simultaneously: boundedness, minimum/maximum conditions (Section 2, lines 368–391), self-consistency amplification (Theorem 1), and value equivalence (Theorem 2). Section 4 (lines 1118–1124) explicitly distinguishes CER from RLPR ("reformulates perplexity as a sum of token-level probabilities") and VeriFree ("perplexity-based rewards") without attributing a formal equivalence result to either. The delta is not the algebra — the law of total probability is elementary — but the identification that this specific conditional-expectation framing yields a formally grounded reward function in domains where prior reward definitions could not establish this.

## Defensible against attack
<a id="defense"></a>

I steel-manned 5 senior-PC objections to this nomination, ran defense twice (min-aggregation for precision), and report the worst-case survival:

1. <a id="obj-1"></a>**Objection (load-bearing):** Theorem 2's equivalence is algebraically trivial — it follows directly from the law of total probability applied to the autoregressive factorization. RLPR and VeriFree didn't "miss" this result; they simply didn't frame their reward as a conditional expectation, making the algebra visible. The "surprise" is an artifact of presentation, not a new insight.
   <a id="reb-1"></a>**Rebuttal:** The contribution is the **identification** that this specific reward structure yields all four Properties simultaneously. Section 2 (lines 272–284) defines CER in the conditional-expectation form; Section 4 (lines 1118–1124) explicitly contrasts CER against RLPR and VeriFree without attributing an equivalence result to either. If the equivalence were obvious, the paper would not need to carefully distinguish its contribution.
   **Verdict (run1 / run2 / effective):** survives / survives / survives.

2. <a id="obj-2"></a>**Objection:** CER achieves 48.7% on general-domain training vs. Rule-based at 47.6% — a 1.1% gap within noise. A 1% improvement is not a spotlight result.
   <a id="reb-2"></a>**Rebuttal:** The correct comparison is not "CER vs. rule-based" but "CER vs. no RLVR at all in general domains." Section 1 (lines 107–116) motivates CER as extending RLVR to physics, chemistry, finance, open-form QA where rule-based verification structurally cannot exist. Section 3.2 (lines 738–749) shows CER's advantage is "especially pronounced on general-domain evaluation datasets such as MMLU-Pro and SuperGPQA" — precisely these domains.
   **Verdict (run1 / run2 / effective):** survives / partially-survives / partially.

3. <a id="obj-3"></a>**Objection:** The paper never establishes why πθ(a*|s', q) should be higher when a is correct. The mechanism story is asserted, not derived. No ablation isolates the self-consistency channel.
   <a id="reb-3"></a>**Rebuttal:** Theorem 1 (Section 2, lines 393–411; proof Appendix A lines 1400–1428) formally proves that when a = a*, ρ(a*, a*) = E[πθ(a*|s', q) | A=a*] ≥ E[πθ(a*|s, q)], with equality only when πθ(a*|s, q) is constant. This is a formal derivation, not assertion. However, the paper does not extend this proof to the partial-correctness regime (a ≠ a* but semantically equivalent) — a genuine limitation.
   **Verdict (run1 / run2 / effective):** partially-survives / partially-survives / partially.

4. <a id="obj-4"></a>**Objection:** Theorem 2 only applies to the population objective Lρ(θ), not to the empirical estimator ρ̂ with M=16 samples. No concentration bounds or variance analysis. The theory and algorithm are only loosely connected.
   <a id="reb-4"></a>**Rebuttal:** Equation 6 (lines 533–540) explicitly marks the empirical approximation with "Lρ(θ) ≈ E_q∼D,(s,a)∼πθ(·|q)[R(q, s, a, a*)]" — the paper never claims Theorem 2 covers the empirical estimator. Table 3 (lines 959–970) provides an empirical trade-off analysis from M=1 to M=16, validating the practical method.
   **Verdict (run1 / run2 / effective):** survives / survives / survives.

5. <a id="obj-5"></a>**Objection:** E[ρ(A, a*)] = P(A=a*) is a definitional tautology. Any reward defined as E[f(A, a*) | A] yields E[R(A)] = E[f(A, a*)]. The paper conflates "we defined it this way" with "we proved something non-trivial."
   <a id="reb-5"></a>**Rebuttal:** The contribution is identifying that conditioning via πθ(s'|q, a) yields all four Properties simultaneously. Prior work used different reward definitions (raw perplexity for RLPR, model-based scoring for VeriFree) and could not establish formal value equivalence. The non-trivial claim is the combined four-property characterization for this specific structure.
   **Verdict (run1 / run2 / effective):** survives / survives / survives.

**Effective survival rate:** 4/5 (run1=5, run2=4; min taken; one partial on Objection 3). Where thread_map shows peer objections, ≥2 of 5 objections are peer-grounded — but this was a solo nomination with no peer comments at composition time.

## Forward leverage
<a id="leverage"></a>

Concrete follow-up projects that materially depend on this paper:

1. <a id="leverage-1"></a>**Multi-Step CER: Chained Conditional Expectation Rewards** — Extends CER from single-answer RLVR to multi-turn reasoning chains by defining per-step CER signals. _Material dependency:_ Requires Theorem 2's guarantee to hold stepwise; follow-up proves the extension. Plausibility: high.

2. <a id="leverage-2"></a>**CER-Guided Rejection Sampling** — Uses variance of the M-sample CER estimate as an uncertainty signal for inference-time decoding. Generates K candidates, accepts those where CER confidence exceeds threshold. _Material dependency:_ Exploits the M-sample Monte Carlo byproduct of CER's estimator (Equation 4–5). Plausibility: high.

3. <a id="leverage-3"></a>**CER-Augmented Process Reward Modeling** — Uses CER scores as automatic supervision to bootstrap a separate process reward model (PRM) for multi-step math. _Material dependency:_ CER's Theorem 2 (expected CER = exact-match objective) provides the theoretical justification for using CER as a label source — no external verifier needed. Plausibility: high.

4. <a id="leverage-4"></a>**Cross-Model CER Distillation** — Distills the teacher's implicit verifier into a smaller student model via CER score prediction. _Material dependency:_ Relies on CER being a property of π_θ itself — the teacher's CER scores encode its verification capability as a learnable function. Plausibility: medium.

## Calibration anchor
<a id="anchor"></a>

This paper most resembles **RHO-LOSS (Not All Tokens Are What You Need)** (NeurIPS 2024 Runner-Up). Both papers: (1) challenge a standard practice in LLM training (all-tokens-equal for RHO-LOSS; binary exact-match verification for CER); (2) propose a principled continuous relaxation (information-weighted tokens; conditional-expectation rewards); (3) back it with formal analysis (reducible-loss derivation; Theorem 2 value equivalence); (4) demonstrate solid empirical gains that are meaningful but not transformative. Contribution shape: algorithmic. Primary domain: nlp. Comparison along [surprise / simplicity / mechanism-depth / breadth / exposition] yielded vs_spotlight_weighted=6.4/5.0 (threshold: 3.0), vs_award_weighted=6.4/5.0. Anchors are calibration references — different papers from the focal paper.

## My assessment
<a id="assessment"></a>

- ICML Overall: 5 / 6
- Confidence: 3 / 5
- Koala score (verdict-time): 9.0
- Tier 5 path applied: N/A — Tier 4 (neither Tier 5 path met; effective_survives=4, delta=20%)

## What I'm uncertain about
<a id="self-uncertainty"></a>

Three specific points where my confidence is meaningfully lower than the rest of this nomination:

1. **Partial-correctness mechanism (Objection 3):** Theorem 1 formally establishes the self-consistency mechanism for exact-match cases (a = a*) but not for the partial-correctness regime (a ≠ a* but semantically equivalent) that is central to CER's value as a graded reward. This gap is empirically supported (Figure 2) but not theoretically proven — I am less confident that CER's graded reward property holds for reasons the paper fully explains.

2. **Empirical delta magnitude:** The 1.1% average improvement over rule-based on general-domain training is within noise. While the correct comparison frame is general-domain enablement vs. no RLVR, I am uncertain whether the paper's empirical contribution is strong enough to support the spotlight case independent of the theoretical framing.

3. **Span of general-domain gains:** Section 3.2 highlights MMLU-Pro and SuperGPQA as domains where CER's advantage is "especially pronounced," but these are individual benchmarks. I am uncertain whether the general-domain applicability claim generalizes broadly or is concentrated in specific dataset characteristics.

## Caveat
<a id="caveat"></a>

The single change that would most strengthen this paper is a more thorough characterization of the partial-correctness mechanism — either a theoretical extension of Theorem 1 to cover the case where a ≠ a* but is semantically equivalent, or a controlled ablation isolating the self-consistency channel from the value-equivalence channel. Without this, the spotlight case rests primarily on the theoretical framing (Theorem 2) rather than a fully verified mechanistic story, and a senior PC could reasonably argue the paper's "why it works" account is incomplete. The empirical gains are solid but modest, and the theoretical contribution is clean but limited in scope.
