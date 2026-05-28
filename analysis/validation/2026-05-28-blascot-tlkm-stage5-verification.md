---
template: manual-verification
stage: 5
date: 2026-05-28
author: Taylor Blascot
company: PT Telkom Indonesia (Persero) Tbk
ticker: TLKM
source-workbook: models/builds/2026-05-23-blascot-tlkm-financials.xlsx
source-spec: docs/specs/2026-05-28-blascot-tlkm-spec.md
---

# Stage 5 Manual Verification — PT Telkom Indonesia (TLKM)

> **Instructions:** Open `models/builds/2026-05-23-blascot-tlkm-financials.xlsx` and
> recompute each ratio below by hand from the named-range cell values in the
> financial-statement tabs. Do **not** read from the Ratios tab — pull raw inputs only.
> Enter your arithmetic in "Manual value". Copy the LLM's stated figure from
> `deliverables/2026-05-28-blascot-tlkm-llm-raw.md` into "LLM's value".
> A discrepancy with a documented cause earns full credit; an unexplained divergence does not.
>
> **⚠ Denominator convention (spec Section 5):** primary return ratios use
> **start-of-year (FY2024)** denominators, not averages. The spec provides `[avg]`
> variants as secondary cross-checks only. Verify the LLM reported start-of-year as
> the headline figure.

---

## Verification Table

The six ratios below were selected because they target the three highest-risk error
modes in this specific spec: **(A)** denominator-timing substitution (average for
start-of-year), **(B)** cross-statement composition (INC_depreciation vs. CASH_DA),
and **(C)** a known reconciliation gap and a unit trap flagged explicitly by the spec.

| Ratio | Formula (named-range notation) | Manual value | LLM's value | Match? | Note |
|---|---|---|---|---|---|
| Return on Assets — start-of-year *(error mode A)* | `currentYear_after_tax_operating_income / startYear_total_assets` | | | | |
| Return on Equity — start-of-year *(error mode A)* | `INC_net / startYear_equity` | | | | |
| Cash Coverage Ratio *(error mode B)* | `(INC_ebit + INC_depreciation) / INC_interest_expense` | | | | |
| Du Pont ROE — four-factor *(error mode C: known gap vs. direct ROE)* | `RATIO_leverage × RATIO_asset_turnover × RATIO_operating_profit_margin × RATIO_debt_burden` | | | | |
| Market Capitalization / MVA *(error mode C: unit trap)* | `share_price × shares_outstanding` (share_price = 3.480 Rp thousands, NOT 3,480) | | | | |
| Economic Value Added (EVA) *(error mode C: multi-step derived)* | `currentYear_after_tax_operating_income − (cost_capital × startYear_total_capitalization)` | | | | |

---

## Why these six

| Ratio | What the LLM is likely to get wrong |
|---|---|
| ROA (start-of-year) | Default to `avg_total_assets` (9.86%) instead of `startYear_total_assets` (9.80%) |
| ROE (start-of-year) | Default to `avg_equity` (16.05%) instead of `startYear_equity` (15.86%) |
| Cash Coverage | Use EBITDA from Cash Flow tab instead of `INC_depreciation` from IS; or confuse with Times Interest Earned (6.66×) |
| Du Pont ROE | Flag the 16.04% vs. 15.86% gap as an *error* and try to force a match — spec V3 says the gap is expected |
| Market Cap / MVA | Use `share_price` = 3,480 (Rp per share) instead of 3.480 (Rp thousands) → 1,000× inflation |
| EVA | Miscalculate `currentYear_after_tax_operating_income` = `INC_net + (1 − tax_rate) × INC_interest_expense`, or use wrong year for total capitalization |

---

## Discrepancy Notes

*For each row where Match? = No: (1) likely cause, (2) whether the LLM followed*
*spec instructions or substituted a default, (3) corrected value used in final analysis.*

<!-- Add one sub-section per discrepancy, e.g.:
### ROA discrepancy
**LLM's value:**   **Your value:**
**Cause:**
**Spec instruction the LLM ignored:**
**Correction applied in final analysis:** yes / no — [how]
-->
