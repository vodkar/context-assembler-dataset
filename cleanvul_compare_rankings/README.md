# CleanVul compare-rankings dataset

One dataset per context-ranking strategy, all built from the same samples in a
single pass so the strategies compare directly.

## Top-level files (rebuilt 2026-10-08)

v2's 710 CleanVul score-4 samples (355 audited vulnerable/fixed pairs, same ids
and order as `../v2/`), 4096-token budget, per-root ROOT / CONTEXT layout,
static findings, max call depth 3. Built with llm_scanner `main` at `2487105`,
which makes builds deterministic (identical runs give identical files) and
applies the budget to the rendered text, markers and headers included: no
sample exceeds 4096 estimated tokens (`len(text) // 3`) except the 6 whose
roots alone are larger (roots are never truncated). `dummy` keeps the fetch
order, which is now depth, file, line, id (nearest code first, source order).

| File | Strategy |
|---|---|
| `cleanvul_context_benchmark.json` | `current` (best-tuned coefficients) |
| `cleanvul_context_benchmark_current_default.json` | `current`, default coefficients |
| `cleanvul_context_benchmark_current_last.json` | `current`, last-trial coefficients |
| `cleanvul_context_benchmark_cpg_structural.json` | `cpg_structural`, best-tuned |
| `cleanvul_context_benchmark_cpg_structural_last.json` | `cpg_structural`, last-trial |
| `cleanvul_context_benchmark_evidence_budgeted.json` | `evidence_budgeted`, best-tuned |
| `cleanvul_context_benchmark_evidence_budgeted_last.json` | `evidence_budgeted`, last-trial |
| `cleanvul_context_benchmark_multiplicative_amplification.json` | `multiplicative_amplification`, best-tuned |
| `cleanvul_context_benchmark_multiplicative_amplification_default.json` | `multiplicative_amplification`, default coefficients |
| `cleanvul_context_benchmark_multiplicative_amplification_last.json` | `multiplicative_amplification`, last-trial |
| `cleanvul_context_benchmark_depth_repeats_context.json` | `depth_repeats_context` |
| `cleanvul_context_benchmark_random_picking.json` | `random_picking` (seed 42) |
| `cleanvul_context_benchmark_dummy.json` | `dummy` |
| `cleanvul_entries.json` | Source CleanVul entries per sample id |

Roots are identical to `../v2/` in every file. Contexts differ from `../v2/`,
which was built at `c123fb7`, before the determinism and budget fixes.

Earlier top-level builds (the July 600-sample flat-layout set, and the
2026-10-07 build at `c123fb7`) are in this repository's history.

## `context_sizes/`

Older builds (1k–16k budgets, June–July 2026, pre-v2 sample set and layout),
kept as they were.
