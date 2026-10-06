# Context-assembler dataset v2, CleanVul score 3 + 4 (6144-token budget)

`../v2_score34/` built with a 6144-token context budget: the same 880 samples
(710 score-4 + 170 score-3), same ids and order, same recipe.

Built with llm_scanner `main` at `c123fb7`.

- Roots and root static findings are identical to `../v2_score34/`; only the
  context differs. Median context length: 2.4K -> 2.5K characters
  (`cpg_structural`) and 5.7K -> 7.3K (`multiplicative_amplification`).
- Score-3 caveats, the id scheme and the excluded score-3 commits are as in
  `../v2_score34/README.md` (no pair exceeds 6144 tokens that fitted in 4096,
  so the sample set is unchanged).

| File | Description |
|---|---|
| `context_assembler_cpg_structural.json` | `cpg_structural` ranking |
| `context_assembler_multiplicative_amplification.json` | `multiplicative_amplification` ranking |
| `cleanvul_python_matched.json` | Function-only baseline for the same 880 samples |
