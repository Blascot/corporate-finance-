# Stages 2–5 — Instructor Review (late-grading from repo)

**Student:** Taylor R Blasco
**Company:** Telkom Indonesia (TLKM) — IFRS, IDR
**Repo:** https://github.com/Blascot/corporate-finance-
**Reviewed:** 2026-05-29

---

## What this PR is

Taylor, you have substantial Stage 2, 3, 4, and 5 deliverables in your repo, but none of them were submitted via Lamaku — so the original grading passes missed them. I've now graded what's in the repo and recorded the scores on my side. This file is the per-stage feedback summary.

**This is feedback only — no scores in the file.** The four grades are on my side; see the email/internal grade reports for the numbers.

The headline: Stages 2, 3, and 4 are *strong* work — Stage 4 in particular is one of the better specs in the cohort. Stage 5 is the one stage where the execution didn't happen — the file skeletons were created on the deadline (2026-05-28) but the substantive content was never filled in. I've applied the Stage 5 floor accordingly.

---

## Stage 2 — Company Selection Memo

**File reviewed:** `docs/decisions/2026-05-17-blascot-tlkm-selection.md` (~744 words)

| Signal | Read |
|---|---|
| Six required sections | All present (Exec Summary, Background, Method/Hypotheses, Findings, Implications, References) |
| Hypotheses | Three falsifiable, in "I expect X because Y" form with quantitative anchors |
| Sources | Real, named, with access dates |
| Audience pitch | Managing-director audience well-pitched |
| Word count | 744w (target 400–600 — slight overrun, not material) |

**What stood out.** The three hypotheses are written in the gold-standard "I expect X because Y" form with quantitative anchors, which is what the rubric is designed to reward. Sources are real (annual report, regulatory filings) with access dates — most students gestured at "company filings" without the specific page citations or access dates. The TLKM selection rationale (Indonesia's largest telco, IFRS-reporting, manageable disclosure regime) reads as a deliberate analytical choice, not a fallback.

**Minor refinement (not affecting score):** the 744-word count is ~25% over the 400–600 target. The "Background" and "Findings" sections could each tighten by ~80 words without losing substance — they currently read as analyst essays where a memo wants telegraphic compression.

---

## Stage 3 — Populated Ratio Workbook

**File reviewed:** `models/builds/2026-05-23-blascot-tlkm-financials.xlsx`

| Signal | Read |
|---|---|
| Tab structure | 6 tabs (Cover, Balance Sheet, Income Statement, Cash Flow, Ratios, Notes) |
| Named ranges | 87 — comprehensive coverage |
| Formula cells | 51 — all resolve cleanly, **0 errors** |
| Both fiscal years | Both FY2025 and FY2024 populated on Balance Sheet |
| Ratio categories | All 6 categories computed |
| Du Pont | Resolves to 16.04% ROE and 9.80% ROA |
| Unit discipline | `share_price = 3.480` (Rp thousands), market cap correctly in IDR billions |

**What stood out.** The unit handling on `share_price` (Rp thousands, not raw IDR) is exactly right and is the kind of catch that most submissions get wrong on the first pass — when share price is in thousands and shares outstanding in millions, the market cap arithmetic produces a 1,000× error if either side is mis-scaled. You got it right.

The 87 named ranges with zero formula errors is publication-grade. The workbook would be immediately usable by another analyst.

**Minor refinement (not affecting score):** Cover and Notes tabs could include 1–2 peer benchmarks (Indosat, XL Axiata, Singtel) for the analytical sections that reference "industry-typical." A tab note specifying which standard you're holding (PSAK convergent with IFRS) would close the loop.

---

## Stage 4 — Technical Specification

**File reviewed:** `docs/specs/2026-05-28-blascot-tlkm-spec.md` (~3,557 words)

| Signal | Read |
|---|---|
| Section coverage | All 11 sections present |
| Spec word count | 3,557 — substantive |
| Named-range conventions | Explicit, with prefix table |
| Ratio categories | All 6 (Performance, Profitability, Efficiency, Leverage, Liquidity, Du Pont) |
| Ratios | 30 — above the 25-ratio rubric expectation |
| Validation rules | 7 — including V3 Du Pont/direct ROE reconciliation |
| HIL iteration | Real V3 iteration documented in prompt log with before/after diagnosis |

**What stood out.** The V3 validation rule (Du Pont ROE vs. direct ROE reconciliation) is the kind of analytical guardrail the rubric is designed to reward. Most submissions either omit this check or write it as a pass/fail without explaining the time-mismatch source. Yours documents it with the expected behavior and a forward-looking note about how a Stage 5 LLM should report the gap.

The HIL iteration documented in your prompt log shows real spec-first thinking — you mentally simulated the Stage 5 LLM's execution against the v1.0 draft, identified the V3 gap, and patched the spec before running Stage 5. That's exactly the iteration discipline the criterion is designed to surface.

The 30 ratios with named-range formulas + computed values across all 6 categories is comprehensive. The spec would let a Stage 5 LLM produce a correct analysis with no further guidance.

**Minor refinement (not affecting score):** §10 Strategic Recommendations could mandate a per-recommendation `**Trade-off:**` line (most students who got the highest Stage 5 scores included trade-off framing per rec). Without it the Stage 5 LLM tends to produce recommendations without explicit cost-of-action.

---

## Stage 5 — LLM Analysis + Verification + Retrospective

**Files reviewed:**
- `deliverables/2026-05-28-blascot-tlkm-final-analysis.md`
- `analysis/validation/2026-05-28-blascot-tlkm-stage5-verification.md`
- `deliverables/2026-05-28-blascot-tlkm-spec-retrospective.md`
- `deliverables/2026-05-28-blascot-tlkm-llm-raw.md`
- `docs/decisions/2026-05-28-blascot-stage2-feedback-response.md`

**Headline.** This is the one stage where execution didn't land. Every Stage 5 file in the repo is a template scaffold — the italic placeholder text and section structure are present, but the substantive content (LLM raw output, manual verification numbers, ratio interpretation, recommendations, retrospective verdicts, Stage 2 feedback acceptance/rejection decisions) was never filled in. The file timestamps show they were created on the deadline (2026-05-28) but the actual analytical work didn't happen.

The Stage 5 floor (70) has been applied because there *is* a final-analysis file present, but the absent content means the raw score before the floor is ~19/100.

| File | Status | What's in it |
|---|---|---|
| Final analysis | Template stub | Italic placeholder instructions; no exec summary, no ratio table, no Du Pont arithmetic, no recommendations |
| Verification table | Template stub | Row design + "Why these six" rationale is real and shows good spec-aware thinking; Manual/LLM value columns are empty |
| Spec retrospective | Template stub | Verdicts blank, gaps blank, ratings blank, process feedback blank |
| LLM raw output | Unfilled | The LLM was never actually run against the spec |
| Stage 2 feedback response | Template stub | Acceptance/rejection columns blank |

**What you *did* produce that earned credit.** The "Why these six" column in the verification table reads as real student thinking — denominator-timing risk, the IDR unit trap, the EVA multi-step risk. That column shows you understood what the verification table is *for*, you just didn't execute the verification itself.

**Why the gap matters.** Your Stage 4 spec is good enough that running it through an LLM and completing the Stage 5 templates would have produced one of the stronger cohort submissions. The 30 ratios, the V3 reconciliation rule, the named-range grammar — they all set the LLM up to produce a clean, correct draft. The work was structurally ready. The final execution step is what didn't happen.

---

## Forward note (no further stages — this is the last one)

The course has no more graded stages, but if you want to close the Stage 5 gap for portfolio/learning purposes (separate from grading):

1. **Feed the Stage 4 spec to your preferred LLM** with the prompt: *"Read this spec; produce the analysis it requests; no other context."* Save the raw output to `deliverables/2026-05-28-blascot-tlkm-llm-raw.md`.
2. **Manually verify 5 ratios** from your Stage 3 workbook against the LLM's stated values. The "Why these six" column you already wrote tells you which ratios to target — execute those rows.
3. **Write the evaluated final analysis.** Take the LLM's raw output and annotate it with `[Analyst note]` markers where you correct or extend the LLM's reading. The 6-section structure your template already has is the right scaffold.
4. **Fill the retrospective.** Section-by-section verdicts against your spec; 2–3 gaps with proposed spec fixes; 3/5 or 4/5 effectiveness rating with justification.
5. **Stage 2 feedback response.** Read your Stage 2 memo; identify which feedback items applied; document the accept/reject decisions.

This would take roughly a Saturday afternoon. It would not change the recorded grade, but it would produce the portfolio piece your Stage 2/3/4 work was building toward.

---

## Repo polish (minor)

- **Add a LICENSE file** — `MIT` is the simplest choice and what most cohort repos use.
- **Add `.gitignore`** — at minimum `.DS_Store` and `*~`.
- **GitHub repo description** — the description field is empty; *"BUS-629 corporate finance portfolio — Telkom Indonesia (TLKM) FY2025 ratio analysis"* would be the project-specific version.
- **README** — currently sparse; the project-status table convention (used in several cohort READMEs) would be a 30-minute lift.

These are cosmetic and would not change any recorded grade.

---

## How to handle this PR

- **Merge it** if you want a permanent record of the review on `main`. On the PR page → **Merge pull request** → **Confirm merge**.
- **Close without merging** if you'd rather not keep the review file in `main`. The scores are already recorded on my side independent of merge — this file is purely for your repo history.

*This review is feedback-only — no scores included.* Score numbers live in the internal grade report and the instructor's email.
