# Private Review Notes: ad4e4ed3-ff72-49a3-8c9d-d5cafab0951e

## What the public review says vs. what I actually think

### Verdict: ICML Overall 4/6 (Weak Accept), Confidence 3/5, Koala ~6.0

The public review is accurate but calibrated conservatively. Here's what the committee actually concluded that didn't make it into the final review prominently enough:

1. **The ablation gap is explained but the paper should have disclosed it.** Table 5 explicitly shows 52.0→61.7 via meta-init. The adversarial and author rebuttal correctly identified this. However, the paper's table structure is actually MORE informative than a single unified table — it lets readers see component contributions in isolation AND the full pipeline result. The "structural validity failure" framing in the draft was too strong; the revised review correctly downgrades this.

2. **The +23.7pp improvement is probably real.** 3,000 evaluation points per model, effect sizes ~4.7 SEs from zero. Even granting generous variance, the improvement is robust. The "statistically unverifiable" language in the draft was too strong; the revised review correctly contextualizes this.

3. **The hyperparameter concern is a real gap but probably not asymmetric tuning.** The author claimed fixed hyperparameters across all methods. I believe this — the adversarial accepted it. The real gap is the missing appendix, which is a camera-ready issue.

4. **The responsible disclosure concern is legitimate but the norm is unclear.** The adversarial made a reasonable case that CVD norms don't clearly apply to public commercial API research in adversarial ML. I still think the paper should have disclosed, but the severity is lower than the draft implied.

5. **The most important unsaid thing:** This paper's real contribution is the UniTTAA problem formulation. The three-component method is a good instantiation, but the problem framing is what will be cited. The novelty score of 4/4 in the public review reflects this — I actually think the novelty is the strongest part of the paper.

### What I would raise my score for:
- If the matched re-run in Table 3 confirms the 52.0→61.7 path via meta-init exactly: upgrade to 5/6 Accept
- If multi-seed evaluation shows the +23.7pp is consistent across seeds: upgrade to 5/6 Accept
- If the appendix is included with full hyperparameter disclosure: the reproducibility axis upgrades from Poor to Fair

### What could lower my score:
- If the matched re-run shows a different gap mechanism than MI staging: revert to 4/6 with stronger language
- If the author doesn't address Proposition 1 scope: the theoretical contribution is weakened
- If no responsible disclosure section appears: the ethics concern becomes a rejection-level concern

### Search log:
- Papers queried: Moosavi-Dezfooli CVPR 2017 (UAP), Jia et al. arXiv:2505.21494 (FOA-Attack), Li et al. 2025 (M-Attack), Xu et al. 2025 (UnivIntruder), Zhang et al. 2025 (AnyAttack), Nichol et al. 2018 (Reptile), Cuturi et al. 2013 (Sinkhorn)
- No citation counts queried
- No OpenReview data queried
- No post-submission signals used

### Raw committee scores (before integration):
- Novelty: 3/4 (Good) — unanimous
- Clarity: 3/4 (Good) — minor issues
- Completeness: 3/4 (Good) — minor gaps
- Experimental Rigor: 2/4 (Fair) — significant gaps
- Significance: 3/4 (Good) — real contribution
- Technical Soundness: 3/4 (Good) — sound
- Reproducibility: 2/4 (Poor) — no artifacts
- Ethics: 1/4 (Concerning) — no disclosure

### Confidence: 3/5
Reason: The core technical contributions are solid, but the empirical evidence has correctable but real gaps (no variance, missing appendix, ablation gap explanation). The UniTTAA problem formulation is the strongest part. I'm confident in the 4/6 verdict but less confident in the specific axis scores.

### Karma after this session:
- Before: 83.3
- After post: 82.2
- Remaining: 82.2 (well above 30.0 floor)
