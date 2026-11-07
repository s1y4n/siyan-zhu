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
| **Version** | 0.1 |
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

**S0_in Source:** European Central Bank (ECB),
Euro Foreign Exchange Reference Rates,
November 6, 2026. EUR 1 = USD 1.1165.

This is an indicative daily reference rate
that will be updated during Stage 4.

https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/eurofxref-graph-usd.en.html

**R_USD Source:** Federal Reserve Board, H.15
Selected Interest Rates, via FRED (DGS6MO).
The six-month U.S. Treasury constant-maturity
yield was 4.19% on November 5, 2026.

This rate serves as an indicative proxy for
the company's six-month USD investment rate.
The model assumes simple interest using
ACT/360. The rate will be updated with
appropriate market data in Stage 4.

https://fred.stlouisfed.org/series/DGS6MO/

**R_FC Source:** Bank of Finland, Euribor Rates. The six-month Euribor rate was 3.014% on November 5, 2026.

This rate serves as an indicative benchmark for the company's six-month EUR borrowing rate. It uses the ACT/360 convention and will be replaced with an appropriate borrowing quote during Stage 4.

https://www.suomenpankki.fi/en/statistics/interest-rates-and-exchange-rates/euribor-rates/

**K_PUT and K_CALL Assumptions:** Both option strike prices are set equal to the indicative spot rate (S0_in = 1.1165 USD/EUR), making them at-the-money. This follows the Scenario 9 instructions. The strikes will be reviewed when the spot rate is updated in Stage 4. The put option is used to hedge the company's EUR receivable; the call input is retained for the standardized model and potential payable scenarios.

**Covered Interest Rate Parity Note:** The scenario-assigned forward rate (F0_in = 1.1705) may differ from the forward rate implied by the independently sourced spot and interest-rate placeholders. This difference will be documented in the model's parity validation check. Consistent market quotes will be reviewed during Stage 4 before applying a strict parity tolerance.

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

The FX hedging model uses the following assumptions and constraints:

1. **Settlement:** The Austrian research institute will pay the full EUR 5,100,000 receivable in 182 days, with no delays or defaults. The entire payment will be converted into USD.

2. **Interest Rate Convention:** Both USD and EUR interest rates use simple annual interest with the ACT/360 convention (`BASIS = 360`). The rates remain constant throughout the 182-day period.

3. **Market Data:** The spot exchange rate, forward rate, and interest rates are indicative placeholders. Market inputs will be reviewed and updated using appropriate live quotes in Stage 4. Scenario-assigned transaction amounts, maturity, and option premiums remain fixed unless the assignment specifies otherwise.

4. **Transaction Costs:** Bank fees, bid-ask spreads, taxes, and other transaction costs are excluded to simplify the comparison. The model includes the stated option premiums.

5. **Option Premium Treatment:** The put premium is paid upfront and its cost is carried forward to settlement using the USD accumulation factor (`DF_USD`). The net option proceeds include this future-value cost. The EUR put protects the receivable from depreciation while allowing participation in favorable exchange-rate movements.

6. **Covered Interest Rate Parity:** The model calculates an implied forward rate using the USD and EUR interest rates and compares it with `F0_in`. Because the assigned forward rate and independently sourced placeholder rates may not satisfy parity, any discrepancy will be reported rather than automatically treated as a calculation error. Consistent market quotes will be reviewed in Stage 4.

7. **Model Scope:** The model evaluates a single EUR receivable and compares no hedge, a forward hedge, a money-market hedge, and a put option hedge. Credit risk, changes in interest rates before settlement, and additional operational costs are excluded.

The model also excludes dynamic hedging, volatility-based option pricing, formal hedge-accounting treatment, and multi-currency portfolio effects.

---

## 5. Calculation Flow

Described in named-range pseudocode so the logic is portable across Excel, Python, and AI prompts. Formulas are written for a **receivable** exposure; Step 7 shows the sign flips required for a payable.

### Step 1 — Derived inputs

1. `DF_USD` = `1 + R_USD × T_DAYS / BASIS` *(accumulation / growth factor, not a fractional discount factor)*
2. `DF_FC` = `1 + R_FC × T_DAYS / BASIS`
3. `FV_PREM_PUT` = `−PREM_PUT × FC_AMT × DF_USD`
4. `FV_PREM_CALL` = `−PREM_CALL × FC_AMT × DF_USD`

### Step 2 — Forward hedge (certainty benchmark)

- `USD_FWD` = `FC_AMT × F0_in`
- Locked-in at t₀; invariant across the `S_T` grid.

### Step 3 — Money-market hedge (parity check)

The money-market hedge consists of three separate calculations:

1. **Borrow EUR today:** `FC_BORROW = FC_AMT / DF_FC`. This determines the amount of EUR to borrow so that the receivable repays the loan and its interest at settlement.
2. **Convert EUR into USD:** `USD_NOW = FC_BORROW * S0_in`. Convert the borrowed EUR into USD using the current spot exchange rate.
3. **Invest USD until settlement:** `USD_MM = USD_NOW * DF_USD`. Invest the USD proceeds for 182 days using the USD interest rate.

Each calculation must appear as a separate formula cell on the `Money Market` worksheet. The EUR receivable will repay the EUR borrowing at maturity.

**Covered Interest Rate Parity Check:**

- `F_IMPLIED = S0_in * DF_USD / DF_FC`
- `PARITY_GAP = F0_in - F_IMPLIED`
- `USD_PARITY_GAP = USD_FWD - USD_MM`

When spot, forward, and interest-rate inputs are consistent with covered interest rate parity, the forward and money-market hedges should produce approximately equal USD proceeds. Because the current placeholder inputs are not parity-consistent, the model must flag and report the discrepancy rather than incorrectly passing the validation check. The comparison will be reviewed using consistent market quotes in Stage 4.

### Step 4 — Option hedge (floor with upside)

Put-and-hold strategy on the receivable:

- Pay `PREM_PUT × FC_AMT` USD today for a put on FC with strike `K_PUT`.
- At settlement, for each `S_T` on the grid:
  - `USD_PUT(S_T)` = `S_T × FC_AMT` + `MAX(0, (K_PUT − S_T) × FC_AMT)` + `FV_PREM_PUT`
  - Equivalently: `MAX(S_T, K_PUT) × FC_AMT + FV_PREM_PUT`

### Step 5 — Sensitivity table (rows of the grid)

For each `S_T` in `S_T_grid`:

| Column | Output | Formula |
|--------|--------|---------|
| No hedge | `USD_NO_HEDGE(i)` | `S_T(i) * FC_AMT` |
| Forward | `USD_FWD` | constant across rows |
| Money market | `USD_MM` | constant across rows |
| Option (put) | `USD_PUT(i)` | `MAX(S_T(i), K_PUT) * FC_AMT + FV_PREM_PUT` |
| Hedge profit | `HEDGE_PROFIT_k(S_T)` | `USD_k − USD_NO_HEDGE` for each of `k ∈ {forward, money market, option}` — one sub-column per strategy |
| Overall winner (incl. no hedge) | label | `ARGMAX(USD_NO_HEDGE, USD_FWD, USD_MM, USD_PUT)` |
| Best active hedge (excl. no hedge) | label | `ARGMAX(USD_FWD, USD_MM, USD_PUT)` |

### Step 6 — Summary metrics (scalar outputs)

- `USD_FLOOR_PUT` = `MIN(USD_PUT)` across `S_T_grid` *(worst-case put outcome on the grid; payable tab uses `USD_CEILING_CALL = MAX(USD_CALL)` instead)*
- `USD_BASE_k` = `USD_k` evaluated at `S_T = S0_in` for each strategy *(the "baseline" row feeds §8.1)*

*`HEDGE_PROFIT_k` is per-row and lives inside the Step 5 grid, not here.*

### Step 7 — Payable variant (sign flips)

For a payable of `FC_AMT` to be settled in FC at maturity, the model mirrors Steps 2–5 with three substitutions: (a) **buy** the forward instead of sell, (b) **borrow USD today, invest in FC** for the money-market leg, and (c) use a **call on FC** with strike `K_CALL` and premium `PREM_CALL`. The sensitivity winner is the strategy that **minimizes** USD outlay at each `S_T`, not the one that maximizes it.

---

## 6. Sensitivity Plan

- **Grid:** `S_T_grid` spans `S0_in × (1 ± 5%)` in 1% increments → 11 rows (including the baseline).
- **Strategies plotted:** no hedge, forward, money market, option (put for receivable / call for payable).
- **Primary chart:** line chart with `S_T` on the x-axis and USD proceeds (or outlay) on the y-axis. Forward and money-market series are horizontal by construction; no-hedge is a straight line through the origin; option is piecewise-linear with a kink at the strike.
- **Secondary table:** hedge profit vs. no hedge for each strategy, to make the visual intuition numeric.
- **What the chart should communicate:** the trade-off between **certainty** (forward / money-market, flat lines), **optionality** (put / call, kinked payoff), and **naked exposure** (no hedge, unbounded on both sides).

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
| Strategy summary | `USD_FWD`, `USD_MM`, `USD_BASE_k` per strategy, plus `USD_FLOOR_PUT` (receivable) / `USD_CEILING_CALL` (payable) | Table above sensitivity grid | Executive at-a-glance |
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
