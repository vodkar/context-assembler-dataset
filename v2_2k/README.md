# Context-assembler dataset v2, 2048-token budget

The v2 dataset (`../v2/`) rebuilt with a 2048-token context budget instead of
4096. Everything else is identical: the same CleanVul score-4 commits, wrong-label
exclusions, tuned `cpg_structural` / `multiplicative_amplification`
coefficients, max call depth 3, Bandit/Dlint/Semgrep static findings, and the
per-root ROOT / CONTEXT layout described in `../v2/README.md`.

Built with llm_scanner `main` at `c123fb7` (rebuilt 2026-10-06; the first
build, at `cbcd1c8`, is commit `ce0e2f4` of this repository). Same samples,
ids, roots and exclusions; context changed in 117 (`cpg_structural`) and 329
(`multiplicative_amplification`) of 616 samples.

## Samples

616 samples (308 vulnerable/fixed pairs), a subset of v2's 710:

- Sample ids and order are v2's, so join with v2 results on `id`.
- 47 pairs (94 samples) are left out because the builder skips a pair when
  either function's code alone exceeds the token budget. Their commit URLs are
  in `excluded_over_budget_commits.txt`.
- Roots and root static findings are identical to v2; only the context is
  shorter. Median context length at 4096 -> 2048 tokens: 2.0K -> 1.6K
  characters (`cpg_structural`) and 5.5K -> 3.1K (`multiplicative_amplification`).

## Files

| File | Description |
|---|---|
| `context_assembler_cpg_structural.json` | `cpg_structural` ranking |
| `context_assembler_multiplicative_amplification.json` | `multiplicative_amplification` ranking |
| `cleanvul_python_matched.json` | Function-only baseline: v2's file restricted to these 616 samples |
| `excluded_over_budget_commits.txt` | Commits whose pairs exceed the 2048-token budget |
