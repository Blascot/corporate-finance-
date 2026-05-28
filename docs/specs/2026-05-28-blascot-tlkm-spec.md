---
template: spec
purpose: "Technical specification for model-driven projects — defines scope, inputs, formulas, validation, and analysis requirements precisely enough that any competent executor (human or LLM) can produce correct output"
audience: student
fields_required: [title, author, date, version, company, scope, model_architecture, data_inputs, named_ranges, derived_inputs, formulas, validation, analysis_requirements, output_format, references]
naming_convention: "YYYY-MM-DD-{slug}.md"
courses: [BUS-314, BUS-629, FIN-321]
notes: "Originally authored for BUS-629 ratios analysis. Section numbering follows the Stage 4 brief's Required-Spec-Components list (Part A 1–7, Part B 8–11)."
---

# Performance & Financial Ratio Analysis Specification — PT Telkom Indonesia (Persero) Tbk

**Author:** Taylor (Blascot)
**Date:** 2026-05-28
**Version:** 1.1
**Company:** PT Telkom Indonesia (Persero) Tbk — IDX: TLKM / NYSE: TLK

---

## 1. Scope & Objective

This specification defines (a) the Excel ratio model and (b) the analytical work to be performed on PT Telkom Indonesia (Persero) Tbk, the Indonesian state-controlled telecommunications and digital-infrastructure operator.

- **Company:** PT Telkom Indonesia (Persero) Tbk (IDX: TLKM / NYSE: TLK)
- **Fiscal period:** Current year FY2025 (year ended December 31, 2025); prior/start year FY2024 (year ended December 31, 2024, restated per Note 2z).
- **Reporting standard:** Indonesian Financial Accounting Standards (PSAK), IFRS-converged. Where a template-default formula assumes US-GAAP behavior, this spec notes the PSAK/IFRS adaptation (see Validation Rules).
- **Reporting currency / units:** Indonesian Rupiah, stated in **Rp billions** unless noted. Share price is entered in **Rp thousands per share** so that `share_price × shares_outstanding` resolves to Rp billions and stays unit-consistent with the financial statements.
- **Analytical objective:** Produce a complete, defensible ratio analysis across six categories (Performance, Profitability, Efficiency, Leverage, Liquidity, Du Pont), reconcile the Du Pont decomposition, and deliver 3–5 evidence-backed strategic recommendations.
- **Intended audience:** Course instructor (academic evaluation). Tone is analytical and precise; every claim ties to a named ratio value; methodology and conventions are stated explicitly rather than assumed.

---

## Part A — Model Specification

### 2. Model Architecture

The model is a single Excel workbook with five tabs. Data flows one direction: statement tabs → Ratios tab. No ratio is hand-keyed; all ratio outputs are formula-driven.

| Tab | Contents | Role |
|-----|----------|------|
| **Cover** | Title, instructions, color key, named-range convention, source documentation | Reference |
| **Balance Sheet** | Current- and prior-year balance sheet (Rp bn) | Data input |
| **Income Statement** | FY2025 income statement (Rp bn) | Data input |
| **Cash Flow Statement** | FY2025 cash-flow statement (Rp bn) | Data input |
| **Ratios** | Four analyst assumptions, derived inputs, all ratio outputs, Du Pont | Calculation + output |
| **Notes** | Company metadata, data source, assumption justification | Reference |

**Color / formatting conventions:**

- **Yellow background** — data inputs pulled from the 10-K / 20-F (overwrite per company).
- **Light-blue background + blue text** — analyst assumptions (share price, shares outstanding, WACC, tax rate, fiscal years).
- **Green text** — formulas: cross-sheet references and derived calculations (do not overwrite).
- **Gray background** — computed ratio outputs on the Ratios tab (do not overwrite).
- Years are formatted as text ("2025"), currency as `Rp #,##0` with units in headers, percentages to one decimal, multiples as `0.00x`, negatives in parentheses.

**Separation of concerns:** inputs live only on the three statement tabs; the Ratios tab holds assumptions, derived intermediates, and outputs. The executor must preserve this separation — no input value should be redefined inside a ratio formula.

### 3. Data Inputs

Every value below is stated numerically; the executor does **not** look up, infer, or estimate any figure. Currency is Rp billions unless the unit column says otherwise. Year-suffixed named ranges are canonical here to remove any ambiguity about which fiscal year a value belongs to (the naming grammar is defined in Section 4).

**Balance Sheet — Assets**

| Named Range | Source | Value (FY2025) | Value (FY2024) | Unit |
|-------------|--------|---------------:|---------------:|------|
| `BAL_cash_marketable_securities` | Balance Sheet | 35,648 | 35,190 | Rp bn |
| `BAL_receivables` | Balance Sheet | 11,223 | 12,193 | Rp bn |
| `BAL_inventories` | Balance Sheet | 901 | 1,096 | Rp bn |
| `BAL_assets_other_current` | Balance Sheet | 13,994 | 14,601 | Rp bn |
| `BAL_assets_current` | Balance Sheet | 61,766 | 63,080 | Rp bn |
| `BAL_ppe_gross` | Balance Sheet | 405,423 | 388,719 | Rp bn |
| `BAL_accumulated_depreciation` | Balance Sheet | 239,970 | 218,384 | Rp bn |
| `BAL_ppe_net` | Balance Sheet | 165,453 | 170,335 | Rp bn |
| `BAL_intangibles_goodwill` | Balance Sheet | 9,237 | 9,442 | Rp bn |
| `BAL_assets_other` | Balance Sheet | 51,303 | 48,532 | Rp bn |
| `BAL_assets_total` | Balance Sheet | 287,759 | 291,389 | Rp bn |

**Balance Sheet — Liabilities & Equity**

| Named Range | Source | Value (FY2025) | Value (FY2024) | Unit |
|-------------|--------|---------------:|---------------:|------|
| `BAL_debt_due` | Balance Sheet | 30,265 | 32,882 | Rp bn |
| `BAL_accounts_payable` | Balance Sheet | 16,184 | 15,336 | Rp bn |
| `BAL_liabilities_other_current` | Balance Sheet | 27,499 | 28,549 | Rp bn |
| `BAL_liabilities_current` | Balance Sheet | 73,948 | 76,767 | Rp bn |
| `BAL_debt_long_term` | Balance Sheet | 44,646 | 43,986 | Rp bn |
| `BAL_liabilities_other_long_term` | Balance Sheet | 18,628 | 16,432 | Rp bn |
| `BAL_liabilities_total` | Balance Sheet | 137,222 | 137,185 | Rp bn |
| `BAL_common_stock_paid_in` | Balance Sheet | 37,344 | 37,557 | Rp bn |
| `BAL_retained_earnings` | Balance Sheet | 113,193 | 116,647 | Rp bn |
| `BAL_equity_shareholders` | Balance Sheet | 150,537 | 154,204 | Rp bn |

**Income Statement (FY2025)**

| Named Range | Source | Value | Unit |
|-------------|--------|------:|------|
| `INC_sales` | Income Statement | 146,742 | Rp bn |
| `INC_cost_goods_sold` | Income Statement | 48,252 | Rp bn |
| `INC_sga` | Income Statement | 26,193 | Rp bn |
| `INC_depreciation` | Income Statement | 37,649 | Rp bn |
| `INC_ebit` | Income Statement | 34,648 | Rp bn |
| `INC_other_income` | Income Statement | 1,660 | Rp bn |
| `INC_interest_expense` | Income Statement | 5,206 | Rp bn |
| `INC_taxable_income` | Income Statement | 31,102 | Rp bn |
| `INC_taxes` | Income Statement | 6,644 | Rp bn |
| `INC_net` | Income Statement | 24,458 | Rp bn |
| `INC_dividends` | Income Statement | 28,406 | Rp bn |
| `INC_addition_retained_earnings` | Income Statement | (3,948) | Rp bn |

**Cash Flow Statement (FY2025)**

| Named Range | Source | Value | Unit |
|-------------|--------|------:|------|
| `CASH_change_receivables` | Cash Flow | 970 | Rp bn |
| `CASH_change_inventories` | Cash Flow | 195 | Rp bn |
| `CASH_change_other_current_assets` | Cash Flow | 607 | Rp bn |
| `CASH_change_accounts_payable` | Cash Flow | 848 | Rp bn |
| `CASH_change_other_current_liabilities` | Cash Flow | (885) | Rp bn |
| `CASH_change_working_capital_total` | Cash Flow | 1,735 | Rp bn |
| `CASH_operating` | Cash Flow | 63,842 | Rp bn |
| `CASH_capex` | Cash Flow | (22,871) | Rp bn |
| `CASH_asset_sales` | Cash Flow | 229 | Rp bn |
| `CASH_other_investing` | Cash Flow | (3,453) | Rp bn |
| `CASH_investing` | Cash Flow | (26,095) | Rp bn |
| `CASH_change_short_term_debt` | Cash Flow | (9,498) | Rp bn |
| `CASH_change_long_term_debt` | Cash Flow | 0 | Rp bn |
| `CASH_dividends_paid` | Cash Flow | (28,406) | Rp bn |
| `CASH_stock_issues` | Cash Flow | 161 | Rp bn |
| `CASH_financing` | Cash Flow | (37,743) | Rp bn |
| `CASH_net_change` | Cash Flow | 4 | Rp bn |

> **Cash-flow note for the executor.** On the Cash Flow tab the "Net income" and "plus Depreciation" lines are cross-sheet links to the Income Statement (`INC_net` = 24,458; `INC_depreciation` = 37,649). `CASH_operating` = 24,458 + 37,649 + 1,735 = 63,842. If the displayed link cells read 0, treat them as `INC_net` and `INC_depreciation` for any cash-flow check — the operating total already incorporates them.

**Analyst Assumptions (Ratios tab)**

| Named Range | Source | Value | Unit |
|-------------|--------|------:|------|
| `yearCurrent` | Analyst input | 2025 | year |
| `yearStart` | Analyst input | 2024 | year |
| `share_price` | Market data (IDX close, 2025-12-30) | 3.480 | **Rp thousands / share** |
| `shares_outstanding` | Market data | 99,061.024659 | millions |
| `cost_capital` | valueinvesting.io WACC, accessed 2026-05-23 | 0.0956 | decimal (9.56%) |
| `tax_rate` | Effective FY2025 = `INC_taxes` / `INC_taxable_income` = 6,644 / 31,102 | 0.213620 | decimal (21.36%) |

> **Unit discipline — read before computing market cap.** `share_price` is **3.480 (Rp thousands)**, not 3,480. Market capitalization = `share_price × shares_outstanding` = 3.480 × 99,061.024659 = **344,732 Rp bn**. Using 3,480 inflates market cap, MVA, and market-to-book by 1,000×. Keep all monetary outputs in Rp billions.

### 4. Named Range Conventions

The model addresses every value by a named range, never by a cell coordinate. The executor must use these names verbatim in all formulas. The grammar:

| Prefix / Pattern | Meaning | Example |
|---|---|---|
| `BAL_[item]_[yr]` | Balance-sheet line item, suffixed with the four-digit fiscal year | `BAL_assets_total_2025`, `BAL_equity_shareholders_2024` |
| `INC_[item]` | Income-statement item (current year FY2025 only) | `INC_sales`, `INC_ebit`, `INC_net` |
| `CASH_[item]` | Cash-flow-statement item (current year FY2025) | `CASH_operating`, `CASH_capex` |
| `share_price`, `shares_outstanding`, `cost_capital`, `tax_rate` | Analyst assumptions (no prefix) | — |
| `startYear_[item]` | Alias for the **prior-year (FY2024)** balance | `startYear_equity` ≡ `BAL_equity_shareholders_2024` |
| `currentYear_[item]` | Alias for the **current-year (FY2025)** balance or a current-year derived figure | `currentYear_equity` ≡ `BAL_equity_shareholders_2025` |
| `avg_[item]` | Simple mean of start-of-year and current-year | `avg_equity` = `AVERAGE(startYear_equity, currentYear_equity)` |
| `RATIO_[name]` | Key ratios reused in the Du Pont decomposition | `RATIO_asset_turnover`, `RATIO_operating_profit_margin`, `RATIO_leverage`, `RATIO_debt_burden` |

> **Year-suffix discipline.** Wherever a balance-sheet value is cited, the executor must use the year-suffixed form (`BAL_assets_total_2025`) or the unambiguous alias (`startYear_*` / `currentYear_*`). A bare `BAL_assets_total` is under-specified — it does not say which year — and must not appear in a formula.

### 5. Derived Inputs

Computed intermediates on the Ratios tab. `startYear_*` aliases the prior-year (FY2024) balance; `currentYear_*` aliases the current-year (FY2025) balance or a current-year derived figure; `avg_*` is the simple mean.

| Named Range | Formula (named-range notation) | Value |
|-------------|--------------------------------|------:|
| `market_capitalization` | `share_price × shares_outstanding` | 344,732.37 |
| `startYear_equity` | `BAL_equity_shareholders_2024` | 154,204 |
| `startYear_inventory` | `BAL_inventories_2024` | 1,096 |
| `startYear_receivables` | `BAL_receivables_2024` | 12,193 |
| `startYear_total_assets` | `BAL_assets_total_2024` | 291,389 |
| `startYear_total_capitalization` | `BAL_debt_long_term_2024 + BAL_equity_shareholders_2024` | 198,190 |
| `currentYear_after_tax_operating_income` | `INC_net + (1 − tax_rate) × INC_interest_expense` | 28,551.90 |
| `currentYear_daily_sales_average` | `INC_sales / 365` | 402.03 |
| `currentYear_equity` | `BAL_equity_shareholders_2025` | 150,537 |
| `currentYear_cash_marketable_securities` | `BAL_cash_marketable_securities_2025` | 35,648 |
| `currentYear_assets_current` | `BAL_assets_current_2025` | 61,766 |
| `currentYear_liabilities_current` | `BAL_liabilities_current_2025` | 73,948 |
| `currentYear_cost_goods_sold_daily` | `INC_cost_goods_sold / 365` | 132.20 |
| `currentYear_debt_long_term` | `BAL_debt_long_term_2025` | 44,646 |
| `currentYear_working_capital_net` | `BAL_assets_current_2025 − BAL_liabilities_current_2025` | (12,182) |
| `currentYear_assets_total` | `BAL_assets_total_2025` | 287,759 |
| `currentYear_total_capitalization` | `currentYear_debt_long_term + currentYear_equity` | 195,183 |
| `currentYear_liabilities_total` | `BAL_liabilities_total_2025` | 137,222 |
| `avg_equity` | `AVERAGE(startYear_equity, currentYear_equity)` | 152,370.5 |
| `avg_total_assets` | `AVERAGE(startYear_total_assets, currentYear_assets_total)` | 289,574 |
| `avg_total_capitalization` | `AVERAGE(startYear_total_capitalization, currentYear_total_capitalization)` | 196,686.5 |

> **Denominator-timing convention — the single most important rule in this spec.** This model's **primary** return ratios (ROA, ROC, ROE) use **start-of-year** denominators, not averages and not end-of-year. Average-based variants are provided separately and suffixed `[avg]`. The executor must report the start-of-year figure as the headline value and may cite the `[avg]` variant as a secondary cross-check. Inventory turnover and receivables turnover likewise use **start-of-year** stock denominators. Do not silently substitute averages.

### 6. Ratio Definitions & Formulas

All formulas in named-range notation. Values shown are the model's computed outputs and are the numbers the executor must reproduce. Six categories, 30 ratio rows.

**Performance**

| Ratio | Formula | Unit | Value | Interpretation |
|-------|---------|------|------:|----------------|
| Market value added (MVA) | `market_capitalization − currentYear_equity` | Rp bn | 194,195 | Wealth created above book equity |
| Market-to-book | `market_capitalization / currentYear_equity` | x | 2.29 | Market values equity at 2.3× book |
| Economic value added (EVA) | `currentYear_after_tax_operating_income − (cost_capital × startYear_total_capitalization)` | Rp bn | 9,605 | After-tax operating profit above the capital charge |

**Profitability** (primary = start-of-year basis; `[avg]` = average basis)

| Ratio | Formula | Unit | Value |
|-------|---------|------|------:|
| Return on assets (ROA) | `currentYear_after_tax_operating_income / startYear_total_assets` | % | 9.80% |
| Return on capital (ROC) | `currentYear_after_tax_operating_income / startYear_total_capitalization` | % | 14.41% |
| Return on equity (ROE) | `INC_net / startYear_equity` | % | 15.86% |
| ROA `[avg]` | `currentYear_after_tax_operating_income / avg_total_assets` | % | 9.86% |
| ROC `[avg]` | `currentYear_after_tax_operating_income / avg_total_capitalization` | % | 14.52% |
| ROE `[avg]` | `INC_net / avg_equity` | % | 16.05% |

**Efficiency**

| Ratio | Formula | Unit | Value |
|-------|---------|------|------:|
| Asset turnover | `INC_sales / startYear_total_assets` | x | 0.50 |
| Receivables turnover | `INC_sales / startYear_receivables` | x | 12.03 |
| Average collection period | `startYear_receivables / currentYear_daily_sales_average` | days | 30.33 |
| Inventory turnover | `INC_cost_goods_sold / startYear_inventory` | x | 44.03 |
| Days in inventory | `startYear_inventory / currentYear_cost_goods_sold_daily` | days | 8.29 |
| Profit margin | `INC_net / INC_sales` | % | 16.67% |
| Operating profit margin | `currentYear_after_tax_operating_income / INC_sales` | % | 19.46% |

> `RATIO_asset_turnover` (0.50) and `RATIO_operating_profit_margin` (0.1946) are reused in Du Pont.

**Leverage**

| Ratio | Formula | Unit | Value |
|-------|---------|------|------:|
| Long-term debt ratio | `currentYear_debt_long_term / (currentYear_debt_long_term + currentYear_equity)` | % | 22.87% |
| Long-term debt-equity ratio | `currentYear_debt_long_term / currentYear_equity` | % | 29.66% |
| Total debt ratio | `currentYear_liabilities_total / currentYear_assets_total` | % | 47.69% |
| Times interest earned | `INC_ebit / INC_interest_expense` | x | 6.66 |
| Cash coverage ratio | `(INC_ebit + INC_depreciation) / INC_interest_expense` | x | 13.89 |
| Debt burden | `INC_net / currentYear_after_tax_operating_income` | x | 0.857 |
| Leverage ratio | `currentYear_assets_total / currentYear_equity` | x | 1.91 |

> `RATIO_debt_burden` (0.857) and `RATIO_leverage` (1.91) are reused in Du Pont.

**Liquidity**

| Ratio | Formula | Unit | Value |
|-------|---------|------|------:|
| Net working capital to assets | `currentYear_working_capital_net / currentYear_assets_total` | % | (4.23%) |
| Current ratio | `currentYear_assets_current / currentYear_liabilities_current` | x | 0.84 |
| Quick ratio | `(currentYear_cash_marketable_securities + BAL_receivables_2025) / currentYear_liabilities_current` | x | 0.63 |
| Cash ratio | `currentYear_cash_marketable_securities / currentYear_liabilities_current` | x | 0.48 |

**Du Pont**

| Ratio | Formula | Unit | Value |
|-------|---------|------|------:|
| ROA (Du Pont) | `RATIO_asset_turnover × RATIO_operating_profit_margin` | % | 9.80% |
| ROE (Du Pont) | `RATIO_leverage × RATIO_asset_turnover × RATIO_operating_profit_margin × RATIO_debt_burden` | % | 16.04% |

### 7. Validation Rules

The executor must verify each check and report pass/fail with the supporting numbers.

**V1 — Balance sheet balances, both years.** `BAL_assets_total` = `BAL_liabilities_total + BAL_equity_shareholders`. FY2025: 287,759 = 137,222 + 150,537 ✓. FY2024: 291,389 = 137,185 + 154,204 ✓.

**V2 — Du Pont ROA ties to direct ROA.** `RATIO_asset_turnover × RATIO_operating_profit_margin` (9.80%) = direct ROA (9.80%) ✓. Both use start-of-year assets.

**V3 — Du Pont ROE does NOT tie to direct ROE, and that is expected.** Du Pont ROE (16.04%) > direct ROE (15.86%) because the two use different equity timing: the Du Pont `RATIO_leverage` term uses **current-year** equity and assets (`currentYear_assets_total / currentYear_equity`), while direct ROE uses **start-of-year** equity (`INC_net / startYear_equity`). The ~0.18 pp gap is the equity-base timing difference. The executor must report this reconciliation explicitly and must **not** force a match or flag a phantom error. (Du Pont ROE does tie closely to ROE `[avg]` of 16.05%, since current-year equity sits near average equity.)

**V4 — Income-statement waterfall.** `INC_ebit + INC_other_income − INC_interest_expense` = `INC_taxable_income` → 34,648 + 1,660 − 5,206 = 31,102 ✓; `INC_taxable_income − INC_taxes` = `INC_net` → 31,102 − 6,644 = 24,458 ✓.

**V5 — Cash-flow articulation.** `CASH_operating + CASH_investing + CASH_financing` = `CASH_net_change` → 63,842 − 26,095 − 37,743 = 4 ✓.

**V6 — Tax-rate consistency.** `tax_rate` = `INC_taxes / INC_taxable_income` = 6,644 / 31,102 = 21.36% ✓.

**V7 — PSAK/IFRS standard check.** Under PSAK (IFRS-converged), depreciation is reported on the face of the income statement (`INC_depreciation` = 37,649), so the cash-coverage ratio uses `INC_depreciation` directly. The executor must not assume a US-GAAP bundling of depreciation into COGS.

---

## Part B — Analysis Specification

### 8. Analysis Requirements

For each category the executor interprets the values, applies the stated benchmark, and notes cross-category connections. Interpretation must cite specific named ratio values, not generic direction.

- **Performance.** Interpret MVA (Rp 194,195 bn), market-to-book (2.29×), and EVA (Rp 9,605 bn) together. Positive EVA means TLKM earned above its 9.56% cost of capital; reconcile this with the 2.29× market-to-book (the market is pricing in continued value creation). Benchmark: EVA > 0 and market-to-book > 1.0 indicate value creation.
- **Profitability.** Lead with the start-of-year ROA/ROC/ROE (9.80% / 14.41% / 15.86%); cite the `[avg]` variants (9.86% / 14.52% / 16.05%) as a sensitivity check. Connect ROC (14.41%) to cost of capital (9.56%): the ~4.85 pp spread is the source of positive EVA. Benchmark: returns above WACC; telecom-sector ROE commonly mid-teens.
- **Efficiency.** Asset turnover of 0.50× reflects a capital-intensive infrastructure business — interpret alongside the high ROC to show TLKM converts heavy assets into adequate returns via margin, not turnover. Flag the **44× inventory turnover / 8.3 days** as expected for a services/telecom firm carrying minimal inventory (not a data error). Receivables collected in ~30 days is healthy.
- **Leverage.** Total debt ratio 47.69%, long-term debt-equity 29.66%, leverage 1.91×. Times-interest-earned of 6.66× and cash coverage of 13.89× show comfortable solvency. Connect leverage to the Du Pont ROE amplification in Section 9.
- **Liquidity.** Current 0.84, quick 0.63, cash 0.48, and **negative** net working capital (Rp −12,182 bn). Interpret as structurally normal for a cash-generative telecom (large CFO of Rp 63,842 bn, supplier-financed working capital) rather than distress — but state the caveat that a sub-1.0 current ratio warrants monitoring of short-term debt rollover (`BAL_debt_due` = Rp 30,265 bn).
- **Cross-category.** Tie strong CFO and positive EVA to the thin liquidity ratios: TLKM runs lean on current assets because operating cash flow, not balance-sheet liquidity, services obligations.

### 9. Du Pont Decomposition

Decompose ROE into its four levers and identify the primary driver:

`ROE (Du Pont) = RATIO_leverage × RATIO_asset_turnover × RATIO_operating_profit_margin × RATIO_debt_burden`
= 1.91 × 0.50 × 0.1946 × 0.857 = **16.04%**

- Quantify each lever's contribution and name the dominant one. Operating profit margin (19.46%) and leverage (1.91×) are the main supports; asset turnover (0.50×) is the structural drag typical of infrastructure; debt burden (0.857) shows interest costs absorb ~14% of operating income.
- Assess sustainability: is the leverage contribution prudent given 6.66× interest coverage? Is the margin defensible against competitive/regulatory pressure in Indonesian telecom?
- **Reconcile to direct ROE** per Validation Rule V3: explain the 16.04% (Du Pont) vs. 15.86% (direct) gap as an equity-timing difference, not an error. Do not paper over it.

### 10. Strategic Recommendations

Produce **3–5** recommendations. Each must:

- Be triggered by a specific ratio value cited in named-range notation (e.g., "current ratio 0.84 with `BAL_debt_due` Rp 30,265 bn").
- State a concrete action (not "improve efficiency"), an owner-level lever (capital allocation, working-capital policy, debt structure, capex discipline), and the expected ratio movement if acted on.
- Distinguish real signals from accounting artifacts (e.g., negative NWC and 44× inventory turnover are structural, not problems to "fix").
- Acknowledge at least one tension — e.g., dividends of Rp 28,406 bn exceed net income of Rp 24,458 bn, producing a negative Rp 3,948 bn addition to retained earnings (`INC_addition_retained_earnings`), a payout-sustainability question worth a recommendation.

### 11. Output Format

- **Audience & tone:** course instructor; analytical, precise, methodologically explicit. Define every convention used; never assume the reader infers the denominator basis.
- **Structure & order:** (1) Company & data summary with assumptions and PSAK note; (2) Ratio results & interpretation, all six categories in the order above; (3) Du Pont analysis with reconciliation; (4) Strategic recommendations (3–5); (5) LLM evaluation & annotations; (6) Executive justification in the author's own voice.
- **Length target:** roughly 1,500–2,500 words plus ratio tables.
- **Presentation of results:** tabular ratio values with units, each followed by interpretive prose; cite named ranges when referencing inputs; report all monetary figures in Rp billions; show the Du Pont reconciliation arithmetic.
- **Non-negotiables:** report start-of-year returns as headline; keep `share_price` unit discipline; surface (do not hide) the Du Pont-vs-direct ROE gap and the negative-NWC explanation.

---

## References

- PT Telkom Indonesia (Persero) Tbk — SEC Form 20-F / consolidated financial statements, FY2025 (statements of financial position, profit or loss, and cash flows); FY2024 figures restated per Note 2z.
- Reporting standard: Indonesian Financial Accounting Standards (PSAK), IFRS-converged.
- Market data: IDX closing price Rp 3,480 on 2025-12-30; shares outstanding 99,061.024659 million.
- Cost of capital: valueinvesting.io WACC estimate for TLKM.JK (9.56%), accessed 2026-05-23.
- Stage 1 ratios template and Stage 3 populated workbook: `2026-05-23-blascot-tlkm-financials.xlsx`.
