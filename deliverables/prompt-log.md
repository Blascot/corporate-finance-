# Prompt Log

| Date | Stage | Tool | Prompt / Task | Output Used |
|---|---|---|---|---|
| 2026-05-28 | 4 | Claude | Fed Stage 4 brief + spec template (raw URLs) + Stage 3 workbook; prompted to populate Parts A & B in named-range notation with all numeric values from the workbook, then iterate on the Du Pont reconciliation. One HIL pass — see note below table. | Draft spec saved to `docs/specs/2026-05-28-blascot-tlkm-spec.md` (Version 1.1); V3 validation rule rewritten after HIL review. |
| 2026-05-24 | Stage 2 | Claude Code | Rubric audit of Stage 2 memo against `stage2-company-selection-memo.md` grading script: identified structural issues (hypotheses in wrong section, market cap missing, word count ~985 words). Directed Claude Code to restructure memo: move hypotheses into `## Findings`, move ratio categories into `## Implications`, add market cap to `## Background`, trim body prose from ~985 to ~572 words. Reviewed all changes before accepting. | Revised `docs/decisions/2026-05-17-blascot-tlkm-selection.md`; all editorial decisions confirmed by student. |
| 2026-05-23 | Stage 3 | Codex | Populate the Stage 3 ratios workbook for PT Telkom Indonesia using audited FY2025/FY2024 financials, document sources, and sanity-check ratios. | Created `models/builds/2026-05-23-blascot-tlkm-financials.xlsx`; used LLM assistance for workbook mapping, formula entry, source notes, and validation, not as an unsourced data substitute. |
| 2026-05-23 | Stage 3 | Codex | Re-source the TLKM Stage 3 workbook to the official local `TLKM10k.pdf` and verify key statement totals from extracted 10-K text. | Updated workbook source documentation to the local official 10-K; checked revenue, total assets, profit, and operating/investing/financing cash-flow totals against the extracted PDF text. |

---

## Stage 4 HIL Before/After Note — 2026-05-28

**Gap identified.** The first draft's Validation section asserted, as a pass/fail check, that the Du Pont ROE should equal the direct ROE. In the Stage 3 workbook those numbers don't match: Du Pont ROE is 16.04% while direct ROE is 15.86%. A naive spec would push the Stage 5 LLM toward one of two failure modes — forcing a false "match," or flagging a phantom error and "correcting" a number that is actually right.

**Why the spec caused it.** The model mixes denominator timing. Direct ROE uses **start-of-year** equity (`INC_net / startYear_equity`), but the Du Pont `RATIO_leverage` term uses **current-year** equity and assets (`currentYear_assets_total / currentYear_equity`). The ~0.18 pp gap is purely that equity-base timing difference — not a model error. Du Pont ROA *does* tie cleanly (both 9.80%) because every term there is start-of-year.

**What changed.** Validation Rule V3 was rewritten to state the mismatch is *expected*, explain its cause, give the reconciling numbers, and instruct the executor not to force a match. A "denominator-timing convention" callout was added in Section 4 making start-of-year the headline basis, with `[avg]` variants labeled as secondary. The spec version was bumped from 1.0 to 1.1.
