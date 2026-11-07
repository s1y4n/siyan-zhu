<div style="border-top: 6px solid #024731; border-bottom: 1px solid #B2B2B2; padding: 12px 0; margin-bottom: 24px; font-family: 'Open Sans', Helvetica, Arial, sans-serif;">
  <div style="color: #024731; font-weight: 700; letter-spacing: 0.06em; text-transform: uppercase; font-size: 0.85rem;">University of Hawaiʻi at Mānoa · Shidler College of Business</div>
  <div style="color: #000000; font-weight: 700; font-size: 1.25rem; margin-top: 4px;">FIN 321: International Finance and Securities</div>
  <div style="color: #525252; font-weight: 400; font-size: 0.95rem;">FX Transaction Hedging Project — Technical Specification</div>
</div>

<!--
BRAND FORMATTING — applied per docs/_branding/design.json (v1.0.0)
  ┌─ Colors ─────────────────────────────────────────────────────────────┐
  │ Primary green ........... #024731  RGB 2,71,49    Pantone 3435 C     │
  │ Primary black ........... #000000  RGB 0,0,0      Process Black      │
  │ Silver (secondary) ...... #B2B2B2  RGB 178,178,178 Cool Gray 5 C     │
  │ White (secondary) ....... #FFFFFF                                    │
  │ Neutral-600 (muted) ..... #525252  (secondary text, captions)        │
  │ UH-green 700 (hover) .... #013D26  (link hover, pressed state)       │
  │ UH-green 50 (tint) ...... #E6F2EF  (callout backgrounds)             │
  │ Yellow (Excel only) ..... #FFFF00  (input highlight — not brand)     │
  ├─ Typography ─────────────────────────────────────────────────────────┤
  │ Headings (web)  .......... Open Sans Bold (H1/H2) / Semibold (H3/H4) │
  │ Headings (print)  ........ Avenir Bold                               │
  │ Body (web)  .............. Open Sans Regular                         │
  │ Body (print)  ............ Avenir Book                               │
  │ Fallback stack  .......... Helvetica, Arial, sans-serif              │
  │ Monospace  ............... Consolas / ui-monospace                   │
  │ Body minimum size  ....... 10 pt (11–12 pt preferred for print)      │
  │ Leading  ................. 3–5 pt greater than type size (print)     │
  │ Alignment  ............... Flush left, ragged right                  │
  ├─ Accessibility ──────────────────────────────────────────────────────┤
  │ • ADA-compliant contrast ratios for ALL text and UI elements         │
  │ • No red body type                                                   │
  │ • No layouts that are too dark for readability                       │
  │ • No custom palettes or gradients outside the official brand         │
  └──────────────────────────────────────────────────────────────────────┘
  Full brand standard: docs/_branding/design.json · Source: https://manoa.hawaii.edu/brand/
-->

# U.S. Laboratory Instruments Manufacturer (Scenario 9) — FX Transaction Hedge Model · Technical Specification

> <span style="color:#024731; font-weight:700;">Technical specification</span> for the FX transaction hedge model — the named-range contract, calculation flow, and validation checks, precise enough that an AI or a colleague could build (or rebuild) the workbook from this document alone. This spec is the input the AI-assisted build works from.

| Field | Value |
|------|------|
| **Created by** | Siyan Zhu |
| **Updated by** | Siyan Zhu |
| **Date Created** | 2026-11-06 |
| **Date Updated** | 2026-11-06 |
| **Version** | 0.2|
| **LLM Used** (optional) | ChatGPT — specification review and feedback |
| **Role** | Treasury Analyst / FP&A Analyst |
| **Audience** | CFO / Director of Treasury |
| **Companion Workbook** | 	model.xlsx (planned for Stage 3)|

---

## 1. Problem Statement

A U.S.-based manufacturer of laboratory mass spectrometers is owed €5,100,000 by an Austrian research institute, with payment due in 182 days. Since the payment will be received in euros but converted into U.S. dollars, the company faces foreign exchange risk because the euro amount is fixed while the dollar value remains uncertain. For example, if the exchange rate drops from a hypothetical 1.20 to 1.10 USD per euro, the company's dollar receipts would decrease from $6,120,000 to $5,610,000, representing a loss of $510,000 (8.33%) that could negatively affect its budget and cash flow. This specification outlines the framework for comparing four strategies: no hedge, a forward contract, a money-market hedge, and a put option. By analyzing USD proceeds under different exchange-rate scenarios, the model will provide the sensitivity evidence needed to support the company's final hedging recommendation in Stage 5.

---

## 2. Inputs (Known Variables)

All inputs are defined as workbook named ranges so Calculation Flow (§5) can be implemented consistently in Excel or by AI. Data sources and access dates will be recorded in the `Notes & Assumptions` worksheet. Market placeholders, including spot and forward rates, interest rates, and option strikes, will be reviewed in Stage 4. Scenario-assigned amounts, maturity, and option premiums remain fixed.

### 2.1 Core Inputs

| Named Range | Description | Placeholder Value | Unit | Stage 4 Source |
|---|---|---|---|---|
| `FC_AMT` | EUR receivable | 5,100,000 | EUR | Scenario 9 (fixed) |
| `S0_in` | EUR/USD spot rate | 1.1165 (indicative) | USD/EUR | ECB, Nov. 6, 2026 |
| `F0_in` | 182-day forward rate | 1.1705 (indicative) | USD/EUR | Live forward quote or CIP-implied rate |
| `R_USD` | Annual USD interest rate | 4.19% (indicative) | Annual % | Federal Reserve H.15 / FRED DGS6MO, Nov. 5, 2026 |
| `R_FC` | Annual EUR interest rate | 3.014% (indicative) | Annual % | Bank of Finland, 6-month Euribor, Nov. 5, 2026 |
| `K_PUT` | Put option strike | 1.1165 (indicative) | USD/EUR | Set equal to S0_in (ECB spot-rate placeholder) |
| `K_CALL` | Call option strike | 1.1165 (indicative) | USD/EUR | Set equal to S0_in (ECB spot-rate placeholder) |
| `PREM_PUT` | Put premium per EUR | 0.0194 | USD/EUR | Scenario 9 (fixed) |
| `PREM_CALL` | Call premium per EUR | 0.0296 | USD/EUR | Scenario 9 (fixed) |
| `T_DAYS` | Days to settlement | 182 | Days | Scenario 9 (fixed) |
| `BASIS` | Day-count denominator | 360 | Days | ACT/360 assumption |

**Market Data Sources and Assumptions:**

- **`S0_in` (1.1165 USD/EUR):** European Central Bank, November 6, 2026. Indicative daily reference rate, not a market-close quote. [ECB Exchange Rates](https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/eurofxref-graph-usd.en.html)
- **`R_USD` (4.19%):** Federal Reserve H.15 / FRED, six-month Treasury yield, November 5, 2026. Used as an indicative USD investment-rate proxy. [FRED DGS6MO](https://fred.stlouisfed.org/series/DGS6MO/)
- **`R_FC` (3.014%):** Bank of Finland, six-month Euribor, November 5, 2026. Used as an indicative EUR borrowing-rate benchmark. [Euribor Rates](https://www.suomenpankki.fi/en/statistics/interest-rates-and-exchange_rates/euribor-rates/)
- **Option Strikes:** `K_PUT` and `K_CALL` equal `S0_in` (1.1165 USD/EUR), making both options at-the-money.
- **Parity:** The assigned forward rate (`F0_in = 1.1705`) differs from the rate implied by the spot and interest-rate placeholders. The discrepancy is reported in Section 7.

Market placeholders will be reviewed in Stage 4. The `Notes & Assumptions` worksheet will record data sources and access dates.

### 2.2 Derived / Intermediate Values

| Name | Description | Formula / Source |
|---|---|---|
| `DF_USD` | USD accumulation factor | `1 + R_USD * T_DAYS / BASIS` |
| `DF_FC` | EUR accumulation factor | `1 + R_FC * T_DAYS / BASIS` |
| `FV_PREM_PUT` | Future value of put premium at settlement | `-PREM_PUT * FC_AMT * DF_USD` |
| `FV_PREM_CALL` | Future value of call premium at settlement | `-PREM_CALL * FC_AMT * DF_USD` |
| `S_T_grid` | Settlement exchange-rate sensitivity grid | `S_T(i) = S0_in * (0.95 + 0.01 * i), where i = 0, 1, ..., 10` |
| `USD_NO_HEDGE` | USD proceeds without hedging |USD_NO_HEDGE(i) = S_T(i) * FC_AMT|

**Day-count convention:** `BASIS = 360` for both USD and EUR (ACT/360).

---

## 3. Tab Architecture

The Excel workbook will contain nine worksheets to organize the inputs, hedge calculations, sensitivity analysis, and documentation. Each hedge calculation will be separated so the model can be reviewed and audited easily.

| Worksheet | Purpose |
|---|---|
| `Cover` | Introduces Scenario 9, the EUR 5,100,000 receivable, and summarizes the USD proceeds for each strategy. |
| `Legend` | Explains the workbook's color coding, named ranges, units, and abbreviations. |
| `Inputs` | Contains all standardized inputs, including exchange rates, interest rates, option strikes, premiums, and settlement days. |
| `No Hedge` | Calculates the USD proceeds if the company leaves its EUR receivable unhedged. |
| `Forward` | Calculates the fixed USD proceeds using the forward exchange rate. |
| `Money Market` | Shows the three steps of borrowing EUR, converting to USD, and investing USD until settlement. |
| `Put Option` | Calculates the net USD proceeds from using a EUR put option, including the premium's future value. |
| `Sensitivity` | Compares all four strategies across 11 settlement exchange-rate scenarios and includes a line chart. |
| `Notes & Assumptions` | Records market-data sources, dates, calculation conventions, limitations, and validation notes. |

Only designated input cells will be manually edited. Calculation cells will use formulas referencing the standardized named ranges. The Cover and Sensitivity worksheets will display linked results rather than hardcoded values.

---

## 4. Assumptions & Constraints

The model uses the following assumptions and constraints:

1. **Settlement:** The full EUR 5,100,000 receivable is collected in 182 days without delays or defaults and converted into USD.

2. **Interest Rates:** USD and EUR interest rates remain constant and use simple interest with ACT/360 (`BASIS = 360`).

3. **Market Data:** Spot, forward, and interest rates and option strikes are indicative placeholders to be reviewed in Stage 4. Scenario-assigned amounts, maturity, and premiums remain fixed.

4. **Transaction Costs:** Bank fees, bid-ask spreads, taxes, and other transaction costs are excluded, except for the specified option premiums.

5. **Option Premiums:** Put premiums are paid upfront and carried forward to settlement using `DF_USD`. Net put proceeds include this cost, providing downside protection while preserving potential upside.

6. **Covered Interest Rate Parity:** The model compares the implied forward rate with `F0_in`. Because current placeholders are not parity-consistent, discrepancies are flagged and reviewed using consistent quotes in Stage 4.

7. **Model Scope:** The model compares no hedge, forward, money-market, and put-option strategies for one EUR receivable. Credit and settlement risk, dynamic hedging, volatility-based option pricing, hedge accounting, and multi-currency portfolio effects are excluded.

---

## 5. Calculation Flow

All calculations use named ranges rather than cell references. The model evaluates USD proceeds from a EUR receivable.

### 5.1 Derived Inputs

Calculate `DF_USD`, `DF_FC`, `FV_PREM_PUT`, and `FV_PREM_CALL` using the formulas in Section 2.2. Interest uses ACT/360, and option premiums are carried forward to settlement.

### 5.2 No Hedge and Forward Hedge

- `USD_NO_HEDGE(i) = FC_AMT * S_T(i)`
- `USD_FWD = FC_AMT * F0_in`

Unhedged proceeds change with the settlement exchange rate, while forward proceeds remain fixed at $5,969,550 using the assigned forward placeholder.

### 5.3 Money-Market Hedge

Calculate the following in three separate formula cells on the `Money Market` worksheet:

1. **Borrow EUR:** `FC_BORROW = FC_AMT / DF_FC`
2. **Convert to USD:** `USD_NOW = FC_BORROW * S0_in`
3. **Invest USD:** `USD_MM = USD_NOW * DF_USD`

At maturity, the EUR receivable repays the EUR borrowing, while the USD investment represents the hedged proceeds.

**Parity checks:**

- `F_IMPLIED = S0_in * DF_USD / DF_FC`
- `PARITY_GAP = F0_in - F_IMPLIED`
- `USD_PARITY_GAP = USD_FWD - USD_MM`

The workbook must report parity discrepancies rather than treating inconsistent placeholder quotes as formula errors.

### 5.4 Put Option Hedge

The company purchases a EUR put at strike `K_PUT`. The upfront premium is `PREM_PUT * FC_AMT`, and its settlement-date cost is represented by the negative value `FV_PREM_PUT`.

- `USD_PUT(i) = MAX(S_T(i), K_PUT) * FC_AMT + FV_PREM_PUT`

The put establishes a minimum net USD proceeds level while preserving upside when the euro appreciates.

### 5.5 Sensitivity and Summary Outputs

For each scenario `i = 0, 1, ..., 10`, calculate `USD_NO_HEDGE(i)`, `USD_FWD`, `USD_MM`, and `USD_PUT(i)`.

- `HEDGE_PROFIT_k(i) = USD_k(i) - USD_NO_HEDGE(i)` for each active hedge.
- Overall winner: strategy with the highest USD proceeds, including no hedge.
- Best active hedge: highest proceeds among forward, money-market, and put strategies.
- `USD_BASE_k`: each strategy's proceeds at `S_T = S0_in`.
- `USD_FLOOR_PUT = MIN(USD_PUT(i))` across the sensitivity grid.

The full sensitivity grid and chart are specified in Section 6.

### 5.6 Payable Variant (Reference Only)

For a EUR payable, the company would buy EUR forward, borrow USD and invest in EUR for the money-market hedge, or purchase a EUR call. The call's net USD outlay is `FC_AMT * MIN(S_T(i), K_CALL) - FV_PREM_CALL`. The preferred strategy minimizes USD outlay. Scenario 9 models a receivable, so the payable variant is not part of the four-strategy comparison.

---

## 6. Sensitivity Plan

The `Sensitivity` worksheet will compare all four strategies across 11 settlement exchange-rate scenarios.

- **Exchange-rate grid:** `S_T(i) = S0_in * (0.95 + 0.01 * i)`, where `i = 0, 1, ..., 10`. This covers ±5% of the initial spot rate in 1% increments, including the baseline.
- **Strategy comparison:** Calculate `USD_NO_HEDGE(i)`, `USD_FWD`, `USD_MM`, and `USD_PUT(i)` for every scenario using the formulas in Section 5.
- **Comparison chart:** Create one line chart with `S_T(i)` (USD/EUR) on the x-axis and USD proceeds on the y-axis. The forward and money-market lines remain flat, the unhedged line changes linearly, and the put-option line provides a floor with upside potential.
- **Additional outputs:** Include hedge profit versus no hedge and identify both the highest-proceeds strategy overall and the best active hedge for each scenario.

The chart will help the CFO compare guaranteed proceeds, downside protection, option costs, and exposure to favorable or unfavorable exchange-rate movements.

---

## 7. Validation Rules

The completed workbook must pass the following checks before its results are used for the Stage 5 decision memo:

- [ ] **Named Ranges:** All ten required inputs use the standardized names from Section 2.1. Calculations use named ranges rather than hardcoded cell references.
- [ ] **Forward Hedge:** `USD_FWD = FC_AMT * F0_in`. Using the initial scenario values, the result must equal $5,969,550.
- [ ] **Money-Market Hedge:** `FC_BORROW * DF_FC = FC_AMT`, `USD_NOW = FC_BORROW * S0_in`, and `USD_MM = USD_NOW * DF_USD`. Each calculation must appear separately.
- [ ] **Covered Interest Rate Parity:** `F_IMPLIED = S0_in * DF_USD / DF_FC`. Compare the implied forward rate with `F0_in`. A discrepancy greater than 0.05% must be flagged. Because the current placeholder inputs are not parity-consistent, the model must report the difference rather than pass the check. Using the initial placeholders, F_IMPLIED should be approximately 1.123038 USD/EUR and USD_MM approximately $5,727,495.52. The difference from forward proceeds should be flagged as an input-consistency issue.
- [ ] **Put Option:** At `S_T = K_PUT`, proceeds must equal `K_PUT * FC_AMT + FV_PREM_PUT`. The put payoff must provide downside protection while preserving upside after premium costs.
- [ ] **Sensitivity Analysis:** The model must generate 11 scenarios from 95% to 105% of `S0_in`, including the baseline. Forward and money-market proceeds must remain constant across the scenarios.
- [ ] **Workbook Integrity:** Calculated outputs must contain formulas, with no Excel errors such as `#REF!`, `#VALUE!`, or `#DIV/0!`. Sources and assumptions must be documented in the `Notes & Assumptions` worksheet, and cell colors must follow the Legend.

---

## 8. Outputs

| Output | Description | Format | Purpose |
|--------|-------------|--------|---------|
| Input panel | All named-range inputs with units, sources, and access dates | Inputs worksheet | Single source of truth |
| Strategy summary | `USD_FWD`, `USD_MM`, `USD_BASE_k` per strategy, plus `USD_FLOOR_PUT` (receivable) / `USD_CEILING_CALL` (payable) | Cover summary and linked table above Sensitivity grid | Executive at-a-glance |
| Sensitivity table | USD proceeds / outlay for each strategy across `S_T_grid` ± 5% | 	Sensitivity worksheet | Core analytical evidence |
| Hedge-profit columns | `USD_k − USD_NO_HEDGE` for each strategy per row | Sub-table | Isolates hedge value-add |
| Winner / best-hedge labels | `ARGMAX` / `ARGMIN` labels per row | Two label columns | Quick-read decision cue |
| Sensitivity chart | Line chart of USD outcome vs. `S_T` for all four strategies | Embedded chart | Visual comparison |
| Executive summary (Stage 5 decision memo) | 1–2 paragraph narrative with explicit recommendation | Separate memo | Downstream deliverable |

### 8.1 Computed Base-Case Values

Record the base-case outcome at `S_T = S0_in` once the model is built. This block serves as a regression checkpoint for the Stage 3 build and the Stage 4 repopulation.

| Strategy | USD Proceeds (Receivable) | USD Outlay (Payable) | Hedge Profit vs. No Hedge |
|----------|--------------------------:|---------------------:|--------------------------:|
| No hedge | | | — |
| Forward | | | |
| Money market | | | |
| Option (put / call) | | | |

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-11-06 | Siyan Zhu | Initial Stage 2 specification draft |
| 0.2 | 2026-11-07 | Siyan Zhu | Reorganized sections, added validation rules, and clarified model assumptions |
