# Context-assembler dataset v2, CleanVul score 3 + 4 (2048-token budget)

`../v2_score34/` built with a 2048-token context budget: 616 score-4 samples
plus 150 score-3 samples (75 pairs), 766 in total. Everything else follows the
v2 recipe (tuned `cpg_structural` / `multiplicative_amplification` coefficients,
max call depth 3, static findings, per-root ROOT / CONTEXT layout).

Built with llm_scanner `main` at `c123fb7`. Note that `../v2_2k/` was built at
the older `cbcd1c8`, so its score-4 contexts differ from the ones here.

## Samples

- Ids are the ones of `../v2_score34/` (score-4 ids from v2, score-3 ids 711+);
  this set is a subset of it, and roots are identical between the two budgets.
- Left out because a function alone exceeds 2048 tokens (the builder skips the
  whole pair): 47 score-4 pairs (same as `../v2_2k/`,
  `excluded_score4_over_budget_commits.txt`) and 15 score-3 pairs; tensorflow
  is also left out (repository size limit). See `excluded_score3_commits.txt`.
- Score-3 caveats are as in `../v2_score34/README.md`: unaudited labels, and 23
  commits shared with the score-4 part.

## Files

| File | Description |
|---|---|
| `context_assembler_cpg_structural.json` | `cpg_structural` ranking |
| `context_assembler_multiplicative_amplification.json` | `multiplicative_amplification` ranking |
| `cleanvul_python_matched.json` | Function-only baseline for the same 766 samples |
| `excluded_score4_over_budget_commits.txt` | Score-4 commits over the 2048-token budget |
| `excluded_score3_commits.txt` | Score-3 commits the builder skipped, with the reason |
