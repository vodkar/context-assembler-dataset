# Context-assembler dataset v2, 6144-token budget

`../v2/` built with a 6144-token context budget instead of 4096. Same 710
CleanVul score-4 samples (355 pairs), same sample ids and order, same recipe
(wrong-label exclusions, tuned `cpg_structural` / `multiplicative_amplification`
coefficients, max call depth 3, static findings, per-root ROOT / CONTEXT layout).

Built with llm_scanner `main` at `c123fb7`.

- Roots and root static findings are identical to `../v2/`; only the context
  differs. Median context length: 2.3K -> 2.4K characters (`cpg_structural`,
  unchanged for 543 of 710 samples) and 5.6K -> 6.9K (`multiplicative_amplification`).
- `cleanvul_python_matched.json` is a copy of `../v2/`'s function-only baseline.

| File | Description |
|---|---|
| `context_assembler_cpg_structural.json` | `cpg_structural` ranking |
| `context_assembler_multiplicative_amplification.json` | `multiplicative_amplification` ranking |
| `cleanvul_python_matched.json` | Function-only baseline (same 710 samples) |
