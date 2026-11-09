# FX Hedging — Scenario 9: Technical Specification

**University of Hawaiʻi at Mānoa | FIN 321**

**Version:** 0.4 | **Updated:** 2026-11-08

**Scenario:** €5,100,000 EUR receivable due in 182 days.
**Workbook:** `model.xlsx` (planned for Stage 3)

---

## 1. Problem Statement

A U.S. laboratory instruments manufacturer expects €5,100,000 from an Austrian institute in 182 days (assumed settlement: May 7, 2027). Euro depreciation could reduce USD receipts; for example, a decline from 1.20 to 1.10 USD/EUR would cause a $510,000 (8.33%) loss. The model compares no hedge, forward, money-market, and put-option strategies to evaluate USD proceeds and support a hedging recommendation.
---

## 2. Inputs (Known Variables)

All inputs use standardized named ranges for the calculations in Section 5. Market placeholders will be updated in Stage 4, while scenario-assigned values remain fixed. Sources and access dates will be documented in the `Notes & Assumptions` worksheet.

### 2.1 Core Inputs

| Named Range | Description | Placeholder Value | Unit | Stage 4 Source |
|---|---|---|---|---|
| `FC_AMT` | EUR receivable | 5,100,000 | EUR | Scenario 9 (fixed) |
| `S0_in` | EUR/USD spot rate | 1.1144 (indicative) | USD/EUR | Investing.com daily close, Nov. 6, 2026 |
| `F0_in` | 182-day forward rate | 1.1705 (indicative) | USD/EUR | Live forward quote or CIP-implied rate |
| `R_USD` | Annual USD interest rate | 4.19% (indicative) | Annual % | Federal Reserve H.15 / FRED DGS6MO, Nov. 5, 2026 |
| `R_FC` | Annual EUR interest rate | 3.014% (indicative) | Annual % | Bank of Finland, 6-month Euribor, Nov. 5, 2026 |
| `K_PUT` | Put option strike | 1.1144 (indicative) | USD/EUR | Set equal to S0_in (at-the-money assumption) |
| `K_CALL` | Call option strike | 1.1144 (indicative) | USD/EUR | Set equal to S0_in (at-the-money assumption) |
| `PREM_PUT` | Put premium per EUR | 0.0194 | USD/EUR | Scenario 9 (fixed) |
| `PREM_CALL` | Call premium per EUR | 0.0296 | USD/EUR | Scenario 9 (fixed) |
| `T_DAYS` | Days to settlement | 182 | Days | Scenario 9 (fixed) |
| `BASIS` | Day-count denominator | 360 | Days | ACT/360 assumption |

**Market Data Sources and Assumptions:**

- **`S0_in` (1.1144 USD/EUR):** Investing.com, EUR/USD Historical Data, November 6, 2026. Indicative daily closing quote. [EUR/USD Historical Data](https://www.investing.com/currencies/eur-usd-historical-data)
- **`R_USD` (4.19%):** Federal Reserve H.15 / FRED, six-month Treasury yield, November 5, 2026. Used as an indicative USD investment-rate proxy. [FRED DGS6MO](https://fred.stlouisfed.org/series/DGS6MO/)
- **`R_FC` (3.014%):** Bank of Finland, six-month Euribor, November 5, 2026. Used as an indicative EUR borrowing-rate benchmark. [Euribor Rates](https://www.suomenpankki.fi/en/statistics/interest-rates-and-exchange-rates/euribor-rates/)
- **Option Strikes:** `K_PUT` and `K_CALL` equal `S0_in` (1.1144 USD/EUR), making both options at-the-money.
- **Parity:** The assigned forward rate (`F0_in = 1.1705`) differs from the rate implied by the spot and interest-rate placeholders. The discrepancy is reported in Section 7.

---
## 3. Tab Architecture

The Stage 3 workbook will contain nine worksheets:

- `Cover` — Scenario 9 overview and linked strategy summary.
- `Legend` — Colors, named ranges, units, and abbreviations.
- `Inputs` — Editable input values, units, and source references.
- `No Hedge`, `Forward`, `Money Market`, `Put Option` — Separate formula-driven hedge calculations; money-market borrowing, conversion, and investment appear in three distinct cells.
- `Sensitivity` — Eleven FX scenarios, proceeds, hedge profits, winner labels, and comparison chart.
- `Notes & Assumptions` — Market sources, dates, conventions, limitations, and validation notes.

Only designated input cells are editable; all other outputs are formula-linked.

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

- `DF_USD = 1 + R_USD * T_DAYS / BASIS`
- `DF_FC = 1 + R_FC * T_DAYS / BASIS`
- `FV_PREM_PUT = -PREM_PUT * FC_AMT * DF_USD`
- `FV_PREM_CALL = -PREM_CALL * FC_AMT * DF_USD`
- `S_T(i) = S0_in * (0.95 + 0.01 * i)`, for `i = 0, 1, ..., 10` (`S_T_grid`)
- `USD_NO_HEDGE(i) = S_T(i) * FC_AMT`

Interest and premium calculations use ACT/360 (`BASIS = 360`).

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

The Stage 3 workbook must satisfy these checks:

- [ ] **Named Ranges:** All ten required names from Section 2.1 are defined; calculated outputs use formulas rather than hardcoded values.
- [ ] **Forward Hedge:** `USD_FWD = FC_AMT * F0_in` must equal $5,969,550 using the initial placeholders and remain constant across all scenarios.
- [ ] **Money-Market Hedge:** Verify `FC_BORROW * DF_FC = FC_AMT`, `USD_NOW = FC_BORROW * S0_in`, and `USD_MM = USD_NOW * DF_USD` in three separate calculation cells. Expected initial USD_MM is approximately $5,716,722.80.
- [ ] **Interest Rate Parity:** Verify `F_IMPLIED = S0_in * DF_USD / DF_FC`. The parity check passes when `ABS(F0_in - F_IMPLIED) / F_IMPLIED <= 0.0005` (0.05%). The initial placeholders imply approximately 1.120926 USD/EUR, so this check must flag an input mismatch, not pass.
- [ ] **Put Option:** At `S_T = K_PUT`, verify `USD_PUT = K_PUT * FC_AMT + FV_PREM_PUT`. Proceeds must stay at the net floor below the strike and increase above it.
- [ ] **Sensitivity:** Generate exactly 11 scenarios from `0.95 * S0_in` to `1.05 * S0_in` in 1% steps. Include the baseline and verify that forward and money-market proceeds remain constant.
- [ ] **Workbook Integrity:** No formula errors (`#REF!`, `#VALUE!`, `#DIV/0!`); calculated outputs are formula-driven. The `Notes & Assumptions` worksheet records sources and access dates. The Legend defines yellow inputs, blue assumptions, black formulas, and green cross-tab links.

---

## 8. Outputs

| Output | Description / Named Results | Worksheet |
|---|---|---|
| Input panel | All required named inputs, values, units, and sources | `Inputs` |
| Strategy summary | `USD_FWD`, `USD_MM`, `USD_BASE_k`, and `USD_FLOOR_PUT` | `Cover` and `Sensitivity` |
| Parity diagnostics | `F_IMPLIED`, `PARITY_GAP`, and `USD_PARITY_GAP` | `Money Market` |
| Sensitivity table | `S_T_grid` and `USD_NO_HEDGE(i)`, `USD_FWD`, `USD_MM`, `USD_PUT(i)` for all 11 scenarios | `Sensitivity` |
| Hedge-profit columns | `HEDGE_PROFIT_k(i)` for each active hedge versus no hedge | `Sensitivity` |
| Winner labels | Overall highest-proceeds strategy and best active hedge for each scenario | `Sensitivity` |
| Comparison chart | One line chart comparing all four strategies against `S_T(i)` | `Sensitivity` |
| Sources and assumptions | Source dates, rate conventions, exclusions, and validation notes | `Notes & Assumptions` |
| Stage 5 decision memo | Final recommendation supported by the workbook's results | Separate downstream deliverable |

### 8.1 Computed Base-Case Values

The base case uses `S_T = S0_in`. The following results will be populated and verified during the Stage 3 workbook build and used as regression checkpoints when market data is updated in Stage 4. Scenario 9 concerns a EUR receivable; payable outputs are not applicable.

| Strategy | USD Proceeds | Hedge Profit vs. No Hedge |
|---|---:|---:|
| No hedge | To be populated in Stage 3 | — |
| Forward | To be populated in Stage 3 | To be populated in Stage 3 |
| Money market | To be populated in Stage 3 | To be populated in Stage 3 |
| Put option | To be populated in Stage 3 | To be populated in Stage 3 |

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-11-06 | Siyan Zhu | Initial Stage 2 specification draft |
| 0.2 | 2026-11-07 | Siyan Zhu | Reorganized sections, added validation rules, and clarified model assumptions |
| 0.3 | 2026-11-07 | Siyan Zhu | Refined model specification following AI review and documented validation improvements |
| 0.4 | 2026-11-08 | Siyan Zhu | Updated spot-rate placeholder, option strikes, source documentation, and validation checkpoints |
