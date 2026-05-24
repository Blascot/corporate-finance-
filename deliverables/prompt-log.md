# Prompt Log

| Date | Stage | Tool | Prompt / Task | Output Used |
|---|---|---|---|---|
| 2026-05-24 | Stage 2 | Claude Code | Rubric audit of Stage 2 memo against `stage2-company-selection-memo.md` grading script: identified structural issues (hypotheses in wrong section, market cap missing, word count ~985 words). Directed Claude Code to restructure memo: move hypotheses into `## Findings`, move ratio categories into `## Implications`, add market cap to `## Background`, trim body prose from ~985 to ~572 words. Reviewed all changes before accepting. | Revised `docs/decisions/2026-05-17-blascot-tlkm-selection.md`; all editorial decisions confirmed by student. |
| 2026-05-23 | Stage 3 | Codex | Populate the Stage 3 ratios workbook for PT Telkom Indonesia using audited FY2025/FY2024 financials, document sources, and sanity-check ratios. | Created `models/builds/2026-05-23-blascot-tlkm-financials.xlsx`; used LLM assistance for workbook mapping, formula entry, source notes, and validation, not as an unsourced data substitute. |
| 2026-05-23 | Stage 3 | Codex | Re-source the TLKM Stage 3 workbook to the official local `TLKM10k.pdf` and verify key statement totals from extracted 10-K text. | Updated workbook source documentation to the local official 10-K; checked revenue, total assets, profit, and operating/investing/financing cash-flow totals against the extracted PDF text. |
