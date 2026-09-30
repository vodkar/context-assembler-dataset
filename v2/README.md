# Context-assembler dataset v2

Same 710 CleanVul score-4 samples (355 vulnerable/fixed pairs) as v1 in the
repository root. The only difference is how each sample's context is laid out.
Both versions exclude the audited wrong labels, use the tuned
`cpg_structural` / `multiplicative_amplification` coefficients and a 4096-token
budget, and carry Bandit/Dlint/Semgrep static findings with snippet source maps.

Built with llm_scanner `main` at `ce2672e` (root/context split: `1437d72`).

## What changed: per-root ROOT / CONTEXT sections

In v1, `code` was one flat snippet that mixed the functions under analysis
(roots) with related repository code, so nothing told the model which part to
judge. In v2:

- Roots are the depth-0 CPG nodes of the changed functions. Nodes with
  overlapping spans in one file merge into one root, so a sample with several
  changed functions has several roots (228 of 710 samples; up to 12 roots).
- Each context node is attributed to exactly one root: the nearest one through
  the selected code graph. A root's context never contains another root's
  context or any root's own lines.
- `code` renders every root followed by its own context, behind marker lines:

  ```
  # ===== ROOT 1/2: app/views.py:10-42 | code under analysis =====
  <root code>
  # ----- CONTEXT for ROOT 1 | reference only: related callers, callees and definitions -----
  # file: app/utils.py
  <context lines>
  # ===== ROOT 2/2: ...
  ```

  A root with no context has no CONTEXT section.
- New `roots` field: a list of `{file_path, line_start, line_end, code,
  context}` with the same split in structured form (`code` = root only,
  `context` = that root's context only).
- `source_map` and `static_findings[].snippet_line` refer to `code`, including
  the marker lines, which are not mapped to any repository line.

## Files

| File | Description |
|---|---|
| `context_assembler_cpg_structural.json` | `cpg_structural` ranking |
| `context_assembler_multiplicative_amplification.json` | `multiplicative_amplification` ranking |
| `cleanvul_python_matched.json` | Function-only baseline, re-keyed to v2 sample ids (content identical to v1) |

Sample ids differ from v1, so join v1 and v2 on
`(metadata.source_row_ids, label)`, not on `id`.
