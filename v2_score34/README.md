# Context-assembler dataset v2, CleanVul score 3 + 4 (4096-token budget)

v2's 710 score-4 samples plus 170 CleanVul score-3 samples (85 vulnerable/fixed
pairs), 880 samples in total. Same recipe as `../v2/`: tuned `cpg_structural` /
`multiplicative_amplification` coefficients, 4096-token budget, max call depth 3,
Bandit/Dlint/Semgrep static findings and the per-root ROOT / CONTEXT layout.

Built with llm_scanner `main` at `c123fb7`.

## Samples

- `CleanVulContextAssembler-1` .. `-710`: the score-4 samples, identical to
  `../v2/` (same ids, order and content).
- `CleanVulContextAssembler-711` .. `-880`: score-3 samples from
  `vulnerability_score_3.csv`, sorted by commit URL, vulnerable before fixed
  (odd id = vulnerable, next id = its fixed half). The same sample has the same
  id in `../v2_2k_score34/`.
- `metadata.source_file` tells the score apart (`vulnerability_score_4.csv` /
  `vulnerability_score_3.csv`); `metadata.source_row_ids` index into that file,
  so join on `(source_file, source_row_ids, label)` or on `id`.

Caveats for the score-3 part:

- The wrong-label audit (`cleanvul_wrong_labels.json`) covers score 4 only;
  score-3 labels are lower-confidence and unaudited.
- 29 score-3 commits also appear among the score-4 samples (different functions
  of the same commit), so some commits contribute two pairs.
- 6 score-3 commits are left out by the builder's usual rules (5 with a function
  over the token budget, tensorflow over the repository size limit); see
  `excluded_score3_commits.txt`.

## Files

| File | Description |
|---|---|
| `context_assembler_cpg_structural.json` | `cpg_structural` ranking |
| `context_assembler_multiplicative_amplification.json` | `multiplicative_amplification` ranking |
| `cleanvul_python_matched.json` | Function-only baseline for the same 880 samples (score-4 part = `../v2/`'s) |
| `excluded_score3_commits.txt` | Score-3 commits the builder skipped, with the reason |
