
# FX Hedging — Scenario 9: Technical Specification

**University of Hawaiʻi at Mānoa | FIN 321 | Version 0.4 | November 8, 2026**

## 1. Problem Statement

A U.S. laboratory instruments manufacturer expects €5,100,000 from an Austrian research institute in 182 days (assumed valuation: Nov. 6, 2026; settlement: May 7, 2027). EUR depreciation creates uncertain USD proceeds: a decline from 1.20 to 1.10 USD/EUR would reduce receipts by $510,000 (8.33%). The model compares no hedge, forward, money-market, and EUR put strategies to support a treasury recommendation.

## 2. Inputs (Known Variables)

All inputs are Excel named ranges. Market values are indicative and subject to Stage 4 review.

| Named Range | Value | Unit / Source |
|---|---:|---|
| `FC_AMT` | 5,100,000 | EUR; Scenario 9 |
| `S0_in` | 1.1144 | USD/EUR; Investing.com close, Nov. 6 |
| `F0_in` | 1.1705 | USD/EUR; assigned forward placeholder |
| `R_USD` | 4.19% | Annual; FRED DGS6MO, Nov. 5 |
| `R_FC` | 3.014% | Annual; Bank of Finland Euribor, Nov. 5 |
| `K_PUT` | 1.1144 | USD/EUR; at-the-money assumption |
| `K_CALL` | 1.1144 | USD/EUR; at-the-money assumption |
| `PREM_PUT` | 0.0194 | USD/EUR; Scenario 9 |
| `PREM_CALL` | 0.0296 | USD/EUR; Scenario 9 |
| `T_DAYS` | 182 | Calendar days; Scenario 9 |
| `BASIS` | 360 | ACT/360 assumption |

**Sources:** [Investing.com EUR/USD](https://www.investing.com/currencies/eur-usd-historical-data); [FRED DGS6MO](https://fred.stlouisfed.org/series/DGS6MO/); [Bank of Finland Euribor](https://www.suomenpankki.fi/en/statistics/interest-rates-and-exchange-rates/euribor-rates/). Treasury yields and Euribor are rate proxies, not company-specific transaction quotes. Scenario-assigned values remain fixed.

## 3. Tab Architecture

The Stage 3 workbook contains nine sheets: `Cover` (strategy summary); `Legend` (colors, units, definitions); `Inputs` (named inputs); `No Hedge`, `Forward`, `Money Market`, and `Put Option` (separate calculations); `Sensitivity` (11 scenarios and chart); and `Notes & Assumptions` (sources, dates, limitations). Only input cells are editable; outputs are formula-linked.

## 4. Assumptions & Constraints

- Full EUR collection occurs after 182 days without default; interest rates remain constant and use simple ACT/360.
- Put premiums are paid upfront; their USD opportunity cost is carried to settlement. Exclude other fees, spreads, and taxes.
- Market inputs and option strikes are indicative; assigned exposure, maturity, and premiums remain fixed.
- The initial forward quote is not parity-consistent with spot/rate placeholders. Flag discrepancies; exclude credit risk, dynamic hedging, and hedge accounting.

## 5. Calculation Flow

All formulas reference the Section 2 named inputs.

**Derived:** `DF_USD = 1 + R_USD*T_DAYS/BASIS`; `DF_FC = 1 + R_FC*T_DAYS/BASIS`; `FV_PREM_PUT = -PREM_PUT*FC_AMT*DF_USD`; `FV_PREM_CALL = -PREM_CALL*FC_AMT*DF_USD`.

**No hedge:** `USD_NO_HEDGE(i) = FC_AMT*S_T(i)`.

**Forward:** `USD_FWD = FC_AMT*F0_in` (fixed).

**Money market — three separate formula cells:**
1. Borrow EUR: `FC_BORROW = FC_AMT/DF_FC`
2. Convert to USD: `USD_NOW = FC_BORROW*S0_in`
3. Invest USD: `USD_MM = USD_NOW*DF_USD`

The EUR receivable repays the borrowing at maturity. Check `F_IMPLIED = S0_in*DF_USD/DF_FC`, `PARITY_GAP = F0_in-F_IMPLIED`, and `USD_PARITY_GAP = USD_FWD-USD_MM`.

**EUR put:** `USD_PUT(i) = MAX(S_T(i),K_PUT)*FC_AMT+FV_PREM_PUT`. The negative premium adjustment reduces net proceeds.

EUR call (payable reference only): `USD_CALL_PAYABLE(i) = MIN(S_T(i),K_CALL)*FC_AMT-FV_PREM_CALL`. This is included as a reference for foreign-currency payables and is not part of the four active receivable hedging strategies.

**Comparison:** `HEDGE_PROFIT_k(i) = USD_k(i)-USD_NO_HEDGE(i)` for each active hedge. Calculate baseline proceeds `USD_BASE_k` at `S_T=S0_in`, put floor `USD_FLOOR_PUT=MIN(USD_PUT(i))`, the overall highest-proceeds strategy, and the best active hedge.

## 6. Sensitivity Plan

Generate 11 exchange rates: `S_T(i)=S0_in*(0.95+0.01*i)`, for `i=0,...,10` (±5% in 1% steps). Compare all four strategies, hedge profits, and winning strategies. Create one line chart with USD/EUR settlement rate on the x-axis and USD proceeds on the y-axis. Forward and money-market lines remain flat; no hedge is linear; the put has a downside floor.

## 7. Validation Rules

- [ ] All ten required named ranges exist; calculations are formula-driven.
- [ ] Forward proceeds equal **$5,969,550** across scenarios.
- [ ] Money-market steps satisfy `FC_BORROW*DF_FC=FC_AMT` and `USD_MM=USD_NOW*DF_USD`; expected **$5,716,722.80**.
- [ ] Implied forward is approximately **1.120926**. Parity passes only if `ABS(F0_in-F_IMPLIED)/F_IMPLIED <= 0.0005`. Current inputs must flag a mismatch.
- [ ] Put proceeds equal the net floor below `K_PUT` and rise above the strike.
- [ ] Exactly 11 sensitivity rows; forward and money-market values stay fixed.
- [ ] No `#REF!`, `#VALUE!`, or `#DIV/0!` errors. Sources/dates are recorded; Legend identifies yellow inputs, blue assumptions, black formulas, and green cross-sheet links.

## 8. Outputs

The workbook produces a linked `Cover` summary; an 11-row `Sensitivity` table with USD proceeds, hedge profits, winner labels, and a comparison chart; `Money Market` parity diagnostics; and documented inputs and sources.

**Base case (`S_T=1.1144`):** No hedge **$5,683,440**; forward **$5,969,550**; money market **$5,716,722.80**; put **$5,582,404.18**. These preliminary checkpoints will be verified in Stage 3 and used to support the Stage 5 decision memo.

---

## Appendix A — Change Log

| Version | Date | Author | Change |
|---|---|---|---|
| 0.1 | 2026-11-06 | Siyan Zhu | Initial Stage 2 specification draft |
| 0.2 | 2026-11-07 | Siyan Zhu | Reorganized sections, added validation rules, and clarified model assumptions |
| 0.3 | 2026-11-07 | Siyan Zhu | Refined model specification following AI review and documented validation improvements |
| 0.4 | 2026-11-08 | Siyan Zhu | Updated spot-rate placeholder, option strikes, source documentation, and validation checkpoints |
