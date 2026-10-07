# CleanVul compare-rankings dataset

One dataset per context-ranking strategy, all built from the same samples in a
single pass so the strategies compare directly.

## Top-level files (rebuilt 2026-10-07)

v2's 710 CleanVul score-4 samples (355 audited vulnerable/fixed pairs, same ids
and order as `../v2/`), 4096-token budget, per-root ROOT / CONTEXT layout,
static findings, max call depth 3. Built with llm_scanner `main` at `c123fb7`.

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

Roots are identical to `../v2/` in every file. The `cpg_structural` and
`multiplicative_amplification` files are not byte-identical to `../v2/`: the
context builder is not fully deterministic between runs (identical runs differ
for some samples), and here 25 resp. 147 of 710 contexts differ from the `../v2/`
build. Compare strategies within this directory, which share one run.

The previous top-level files (600 samples, flat layout, July 2026) are in this
repository's history before this commit.

## `context_sizes/`

Older builds (1k–16k budgets, June–July 2026, pre-v2 sample set and layout),
kept as they were.
