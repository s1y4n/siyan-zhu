# Scenario 9 — FX Hedging Stage 4 Market-Data Memo

**Prepared:** 2026-11-20 (HST)  
**Data observation date:** 2026-11-20  
**Research access log:** 2026-11-20, approximately 23:22 HST (2026-11-21 09:22 UTC). This is the session timestamp; individual source-access seconds were not retained.  
**Workbook:** `capabilities/fx-hedging/model.xlsx`  
**Prepared for:** FIN 321, Stage 4: Market Data + Population  
**Exposure:** U.S. laboratory instruments manufacturer is owed **EUR 5,100,000** from an Austrian research institute.

## 1. Market inputs and provenance

The model retains the 11-name input contract in `Inputs!B7:B17`. Source and observation date are also entered next to each input in `Inputs!E:G` and in `Notes & Assumptions`.

| Named input | Value entered | Classification | Source and observation date | Provenance / assumption |
|---|---:|---|---|---|
| `FC_AMT` | EUR 5,100,000 | Scenario constant | Assigned Scenario 9 | Receivable, unchanged |
| `S0_in` | **1.0990 USD/EUR** | Market reference proxy | **European Central Bank** daily EUR/USD reference rate, **2026-11-20**; https://www.ecb.europa.eu/stats/policy_and_exchange_rates/euro_reference_exchange_rates/html/index.en.html | ECB fixing based on market observations normally around 14:10 CET, published ~16:00 CET. **Not a literal market-close price or executable transaction quote**. A separately checked Investing.com 2026-11-20 historical close was **1.0968**; the ECB reference was selected for primary-source traceability. |
| `F0_in` | **1.1705 USD/EUR** | Assigned quote / assumption | Scenario 9 / Stage 2 specification (no live bank requote provided) | **Preserved** as instructor/scenario quote; never replaced by the CIP estimate. Stage 4 calls the quote one-year but the model hedge horizon is 182 days; see limitations. |
| `R_USD` | **4.51% = 0.0451** | USD interest-rate proxy | **U.S. Department of the Treasury**, Daily Treasury Par Yield Curve Rates, **1 Yr**, **2026-11-20**; https://home.treasury.gov/resource-center/data-chart-center/interest-rates/TextView?field_tdr_date_value_month=202611&type=daily_treasury_yield_curve | Transparent, official one-year U.S. government yield selected under Stage 4's one-year-rate guidance. Not the borrowing/lending rate actually available to this manufacturer; imperfect for 182 days and mixed in a simple ACT/360 calculation. |
| `R_FC` | **3.372% = 0.03372** | EUR interest-rate proxy | **Bank of Finland**, 12-month Euribor (ACT/360), **2026-11-20**; https://reports.suomenpankki.fi/WebForms/ReportViewerPage.aspx?report=%2Ftilastot%2Fmarkkina-_ja_hallinnolliset_korot%2Feuribor_korot_xml_long_en | Euro unsecured bank money-market benchmark. Selected to satisfy the one-year rate guidance; not a manufacturer-specific euro borrowing rate. |
| `K_PUT` | **1.0990 USD/EUR** | At-the-money assumption | Set equal to selected ECB reference spot on 2026-11-20 | This is **assumed**, not a live dealer option strike quote. |
| `K_CALL` | **1.0990 USD/EUR** | At-the-money assumption | Set equal to selected ECB reference spot on 2026-11-20 | Payable illustration only; not an active receivable hedge. |
| `PREM_PUT` | **0.0194 USD per EUR** | Scenario assumption | Assigned Scenario 9 | **Unchanged**; accessible FX option quotes are unreliable for this project, per instructor. Premium paid today and carried to settlement. |
| `PREM_CALL` | **0.0296 USD per EUR** | Scenario assumption | Assigned Scenario 9 | **Unchanged**; payable reference only. |
| `T_DAYS` | **182 days** | Scenario constant | Assigned Scenario 9 | Preserved per Stage 4 instruction; contract-date and quote-tenor issues noted below. |
| `BASIS` | **360 days/year** | Model convention | Stage 2 specification | ACT/360 simple-interest approximation retained. |

**Spot alternative checked:** Investing.com EUR/USD historical data, November 20, 2026 (close ~1.0968 USD/EUR): https://www.investing.com/currencies/eur-usd-historical-data . This alternative is *not* the workbook input.

## 2. Interest-rate selection rationale

- **USD (`R_USD`, 4.51%)**: The official one-year Treasury par yield was selected because Stage 4 expressly permits a one-year government yield and requires a reproducible dated proxy. It is a sovereign-security yield, not the manufacturer's actual USD investment or borrowing rate.
- **EUR (`R_FC`, 3.372%)**: The 12-month Euribor quote was selected as a transparent euro money-market reference quoted on an ACT/360 basis. It is not the manufacturer's quoted euro credit rate.
- **Comparability limitation**: These instruments have different credit/liquidity characteristics, and both are **one-year** benchmarks applied to a **182-day** modeled exposure. Thus the resulting CIP calculation is a diagnostic proxy, not proof of an exploitable arbitrage opportunity.

## 3. CIP-implied forward, quoted-forward gap

Simple-interest day fraction = `T_DAYS/BASIS = 182/360`.

`F_CIP = S0_in * (1 + R_USD * T_DAYS/BASIS)/(1 + R_FC * T_DAYS/BASIS)`

`= 1.0990 * (1 + 0.0451 * 182/360)/(1 + 0.03372 * 182/360)`

| Measure | Result |
|---|---:|
| Implied forward (`F_CIP`, USD/EUR) | **1.1052168112** |
| Assigned/bank forward (`F0_in`, USD/EUR) | **1.1705000000** |
| Quote minus implied, USD/EUR | **+0.0652831888** |
| Relative discrepancy `(F0_in/F_CIP) - 1` | **+5.9068%** |
| Dollar discrepancy on EUR 5,100,000 | **+$332,944.26** |
| Forward USD proceeds | **$5,969,550.00** |
| CIP-equivalent / money-market USD proceeds | **$5,636,605.74** |

**Parity audit result:** `FLAG: mismatch` remains intentional. The quote is much higher than the simple CIP benchmark. **Do not force this flag to PASS by overwriting `F0_in`.** Ask the instructor for clarification or a bank requote if appropriate, especially given the 1-year quote description vs 182-day horizon. Do not infer an available trading arbitrage from the proxy yields.

## 4. Recalculated base-case hedge proceeds

| Strategy | Model USD proceeds at settlement | Calculation / note |
|---|---:|---|
| No hedge | **$5,604,900.00** | `FC_AMT * S0_in` (spot held at reference rate for base case) |
| Forward | **$5,969,550.00** | `FC_AMT * F0_in` |
| Money market | **$5,636,605.74** | Borrow EUR present value; exchange at spot; invest USD for 182/360 |
| EUR put | **$5,503,704.11** | `FC_AMT * MAX(S0_in,K_PUT)` less USD future value of put premium |
| EUR call (payable reference, **not a fifth strategy**) | **$5,759,301.97 cost** | Illustrative call-hedged EUR payable cost, including future-value premium |

Put premium at inception: EUR 5,100,000 × $0.0194/EUR = **$98,940.00**. At settlement, its simple-interest future value is **$101,195.89**.

**Sensitivity:** Eleven model spots from **1.044050** (−5%) to **1.153950** (+5%) in 1%-of-spot increments. Forward and money-market lines remain flat. Put downside floor remains **$5,503,704.11**. The assigned forward is above the entire +/-5% spot grid, so it wins across all eleven grid cases; this is a consequence of the unusual assigned quote, **not** an input to change.

## 5. Population changes, structural fixes, and checks

**Population** (via original named-range input cells only): Changed `Inputs!B8`, `B10`, `B11`, `B12`, `B13`. Retained `FC_AMT`, `F0_in`, both premiums, `T_DAYS`, and `BASIS`. Updated input/source description fields and the Notes & Assumptions provenance links.

**Structural issue discovered:** The Stage 3 `Cover` expected-value checkpoints for forward, money market, CIP, no hedge, put, and call were **hardcoded to November 6 placeholder outcomes**. Simply replacing spot/rates would have created false `REVIEW` statuses despite correct model formulas. Replaced only **`Cover!C24:C29`** with independent, named-input-based benchmark formulas; existing observed hedge formulas were not changed. The formula integrity audit now checks **257 business formula locations** rather than 251 (the original 251 plus these 6 new benchmark formulas).

**Cover and notes presentation:** Updated market-data snapshot to 2026-11-20; original scenario settlement formula remains linked to the November 6 scenario anchor rather than silently moving the fixed contractual settlement. Updated parity explanatory notes and source labels.

**Validation and Excel confirmation:** 14 base/sensitivity/structural checks displayed **PASS** in the model calculation engine, while the parity check retained the expected **FLAG: mismatch**. The expanded 257-cell `ISFORMULA` audit was recalculated in Microsoft Excel. **Student-confirmed results (2026-11-24 HST):** `Formula Audit!B14=257`, `Formula Audit!E14=PASS`, `Cover!B38=257`, and `Cover!E38=PASS`. This is a student-reported verification. The interactive FX Hedging Lab forward, money-market and CIP results agree to displayed precision; its complete sensitivity grid also matches on spot, unhedged, forward and money-market results. Its put differs by **$2,255.89** because the lab does not carry the premium to settlement; see Section 6.

**Original formula and chart preservation:** The original 11 workbook defined names and four-series sensitivity chart were checked structurally after export. Six expected-benchmark formulas were added, and all previously existing formulas retained their locations and definitions.

## 6. FX Hedging Lab cross-check

Lab: https://adamwstauffer.github.io/ai-lms/fxlab.html

**Evidence:** Student-provided screenshots of the lab's **Hedge results**, **Interest-rate-parity check**, **11-row sensitivity grid**, and **sensitivity chart**, submitted November 24, 2026 (HST), after entering the Stage 4 inputs. Displayed lab numbers are rounded to the nearest dollar or 4 decimals; the workbook and calculations retain full precision. The screenshots allow both matching outputs and the separate put-premium discrepancy to be documented.

| Output | Model (unrounded) | Lab display (screenshot) | Finding |
|---|---:|---:|---|
| Forward proceeds | $5,969,550.00 | $5,969,550 | **MATCH** |
| Money market, Step 1 — EUR borrowed | €5,014,515.8764 | €5,014,516 | **MATCH** (rounded) |
| Money market, Step 2 — USD converted | $5,510,952.9481 | $5,510,953 | **MATCH** (rounded) |
| Money market, Step 3 — USD maturity proceeds | $5,636,605.7370 | $5,636,606 | **MATCH** (rounded) |
| CIP-implied forward | 1.1052168112 | 1.1052 | **MATCH** (4 decimals) |
| Assigned quoted forward | 1.1705 | 1.1705 | **MATCH** |
| CIP discrepancy in USD | **+$332,944.26** (quote minus implied) | **−$332,944** (implied minus quote) | **MATCH in magnitude**; sign convention reversed |
| EUR put proceeds at 0% spot shift | $5,503,704.11 | $5,505,960 | **DIFFERENCE: $2,255.89**, explained by option-premium timing below |
| 11-case sensitivity grid | Spot range 1.04405–1.15395 | Eleven rows from −5% through +5% | **MATCH** for spot, unhedged, forward and money market; put differs by a constant premium carrying cost |

**Parity-warning interpretation:** The lab visibly flags: “Parity gap of $-332,944 — in a real model this means a formula error (wrong rate basis, inverted ratio, or mis-sized loan), not an arbitrage.” This is a **generic lab diagnostic**, not independent evidence of a formula mistake in this workbook. The lab's forward, EUR borrowing, USD conversion, USD investment and CIP equations all agree with the independently recomputed model calculations to the displayed precision. The nonzero gap remains because `F0_in = 1.1705` is the preserved scenario-assigned bank quote, not a market-clearing 182-day executable quote, and the Treasury/Euribor rates are imperfect proxies. **Do not change the original bank quote to clear the warning.** Consult the instructor about a requote and the one-year-versus-182-day tenor mismatch as appropriate.

### Sensitivity grid and put-premium discrepancy (November 24, 2026)

The lab has **11 scenarios**, from −5% to +5% around the selected spot rate of 1.0990. The unhedged values, forward proceeds ($5,969,550), and money-market proceeds (~$5,636,606) agree with the workbook in **all 11 scenarios**, to the lab's displayed-dollar precision. The `BEST` column shows **Forward** for all 11 rows, the same strategy identified in the workbook. The lab rounds its displayed `S_T` rate to four decimals, but uses the unrounded scenario rate in computing unhedged USD proceeds; this display rounding is **not** a formula discrepancy.

The **put proceeds differ by a constant $2,255.89 in every scenario**:

| Shift | Underlying `S_T` (unrounded) | Unhedged USD (both) | Workbook put USD | Lab put USD | Put difference (Lab − workbook) |
|---:|---:|---:|---:|---:|---:|
| −5% | 1.04405 | $5,324,655 | $5,503,704.11 | $5,505,960 | +$2,255.89 |
| −4% | 1.05504 | $5,380,704 | $5,503,704.11 | $5,505,960 | +$2,255.89 |
| −3% | 1.06603 | $5,436,753 | $5,503,704.11 | $5,505,960 | +$2,255.89 |
| −2% | 1.07702 | $5,492,802 | $5,503,704.11 | $5,505,960 | +$2,255.89 |
| −1% | 1.08801 | $5,548,851 | $5,503,704.11 | $5,505,960 | +$2,255.89 |
| 0% | 1.09900 | $5,604,900 | $5,503,704.11 | $5,505,960 | +$2,255.89 |
| +1% | 1.10999 | $5,660,949 | $5,559,753.11 | $5,562,009 | +$2,255.89 |
| +2% | 1.12098 | $5,716,998 | $5,615,802.11 | $5,618,058 | +$2,255.89 |
| +3% | 1.13197 | $5,773,047 | $5,671,851.11 | $5,674,107 | +$2,255.89 |
| +4% | 1.14296 | $5,829,096 | $5,727,900.11 | $5,730,156 | +$2,255.89 |
| +5% | 1.15395 | $5,885,145 | $5,783,949.11 | $5,786,205 | +$2,255.89 |

**Diagnosis:** Both calculations apply the same `MAX(S_T,K_PUT) × FC_AMT` payoff. The lab's **$5,505,960 put floor** equals `1.0990 × 5,100,000 − (0.0194 × 5,100,000)`, which deducts the **upfront nominal premium** of **$98,940** without any interest carrying cost. The workbook's `Put Option!B8` formula is `=−PREM_PUT*FC_AMT*(1+R_USD*T_DAYS/BASIS)`; it carries that premium to settlement at the USD proxy rate, giving a premium future value of **$101,195.89** and net put floor of **$5,503,704.11**. Therefore, the entire lab–workbook put difference is **$101,195.89 − $98,940 = $2,255.89**. This is a model-convention difference, **not an unexplained arithmetic discrepancy**.

**Resolution and treatment:** Retain the workbook's time-adjusted premium formula, because the Stage 2 specification adopts a premium future-value adjustment and the professor's November 16 Stage 3 review explicitly validated the put premium carried to settlement. Do **not** silently change this previously approved model to match the lab's uncarried premium. State this known lab convention difference, including the $2,255.89 amount, in Stage 5 if comparing put results; seek the instructor's clarification only if an exact lab match is required. The lab's chart visually groups the flat forward/money-market family, whereas the workbook retains the two distinct fixed-dollar proceeds.

**Cross-check status:** The complete 11-row grid and forward/money-market/CIP calculations have been compared. The put difference has been isolated, quantified, and explained; the workbook's approved settlement-value convention is preserved. The parity quote mismatch continues to be separately reported and remains unresolved pending an instructor requote or clarification.

## 7. Open issues for Stage 5 / instructor clarification

1. **Quote tenor:** Stage 4 says *1-year bank forward*, while the scenario payoff period is **182 days**. The original quote of 1.1705 is retained because only the instructor may provide a requote.
2. **Dates / scenario horizon:** `Cover!B7` continues to calculate **2027-05-07**, the original contract settlement from its **2026-11-06** scenario start. But **2026-11-20 to 2027-05-07 is only 168 days**. The Stage 4 instruction expressly preserves `T_DAYS = 182`; this memo discloses that the rate-horizon and contract-date assumptions are not fully synchronized. Seek course guidance rather than silently revise the agreement.
3. **Quote basis:** The selected spot is the **ECB reference fix**, not a literal market-close price. Stage 4 accepts ECB fixes as suitable free data, but a strict execution-time close would require switching to a timestamped closing quote and rerunning the analysis.
4. **No dealer transaction prices:** Both rates are benchmarks; the option premiums and forward are assigned. The very large CIP discrepancy should not be treated as a profit opportunity without comparable executable quotes.
5. **Lab put-premium convention:** The November 24 lab sensitivity screenshot confirms a $2,255.89 constant difference in put proceeds because the lab subtracts only the upfront $98,940 premium while the workbook carries it to settlement ($101,195.89). The discrepancy is explained and the Stage 3-approved workbook formula is retained; seek instructor guidance if exact agreement is required. The 257-cell Excel formula audit was confirmed by the student on November 24 HST.

## 8. Suggested version control and AI disclosure

Recommended commit sequence:
1. `Stage 4: Populate November 20 spot, rates and ATM strikes`
2. `Stage 4: Replace stale audit benchmarks with live formulas`
3. `Stage 4: Add sourced market data memo and cross-check record`

Suggested `prompt-log.md` disclosure: ChatGPT helped identify November 20 primary-source data, choose and document rate proxies, populate the five market-input cells, calculate CIP and hedge proceeds, diagnose and replace stale hardcoded audit benchmarks, prepare the memo, and independently examine workbook structure. The student must verify source retrieval, Excel recalculation, and actual FX Lab outputs before submission.
