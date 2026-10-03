# Context-assembler dataset v2

Same 710 CleanVul score-4 samples (355 vulnerable/fixed pairs) as v1 in the
repository root. The only difference is how each sample's context is laid out.
Both versions exclude the audited wrong labels, use the tuned
`cpg_structural` / `multiplicative_amplification` coefficients and a 4096-token
budget, and carry Bandit/Dlint/Semgrep static findings with snippet source maps.

Built with llm_scanner `main` at `cbcd1c8`
(root/context split: `1437d72`).

### Rebuild 2026-10-03: cross-file call resolution and class attributes

The context datasets were rebuilt in place from the same 355 commits with the
same settings; sample ids, labels, roots and root static findings are
unchanged, and `cleanvul_python_matched.json` is untouched. The CPG changed:

- Calls such as `module.f()`, `self.m()` / `super().m()` on a base class in
  another file, and methods called on objects (when the method name is unique
  in the repository) now resolve across files, so their definitions can enter
  the context. Before, 0 of 82 such call sites in a 33-repo audit resolved.
- A class node now includes its class-level attributes (e.g. Django
  `template = ...`, form or dataclass fields), not just the `class X:` header.

Effect: context changed in 502 (`cpg_structural`) and 534
(`multiplicative_amplification`) of 710 samples; median context size went from
1.2K to 2.1K and from 2.7K to 4.6K characters; empty contexts went from 90 to 38.
The previous build is
commit `5cd2de6` of this repository (built at llm_scanner `ce2672e`).

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
