# Stage 3 Build Audit — FX Hedging, Scenario 9

**Student:** Siyan Zhu  
**Course:** FIN 321, University of Hawaiʻi at Mānoa  
**Audit date:** November 14, 2026  
**Specification:** `capabilities/fx-hedging/spec.md` (Version 0.6)  
**Workbook:** `capabilities/fx-hedging/model.xlsx`

## Purpose and approach

I used ChatGPT to help build the Excel workbook from my Stage 2 specification. I then opened it in Excel and checked named ranges, formulas, sensitivity behavior, money-market calculations, formatting, and validation results. I did not assume that the AI-generated file was correct just because it opened. Below are the findings from the checks I performed.

## Finding 1 — Named ranges and interest-rate precision

**What I checked:** I opened **Formulas → Name Manager** and compared the named input ranges with Section 2 of my specification. I also checked the underlying value of the foreign-currency interest rate.

**What I found:** All **11** named inputs were present and pointed to the `Inputs` worksheet. The `R_FC` cell displayed a rounded percentage in some views, but its actual stored value was **3.014%**, as specified.

**What I did:** I kept the input values and named-range definitions unchanged. I confirmed that the displayed rounding did not mean the input had been reduced to 3.01%.

## Finding 2 — Formulas and sensitivity recalculation

**What I checked:** I selected `Sensitivity!C12` and verified that its formula was `=FC_AMT*B12`, rather than a typed result. I temporarily changed `S0_in` from **1.1144** to **1.1000** to see whether the workbook recalculated.

**What I found:** The sensitivity rates, no-hedge proceeds, money-market proceeds, and chart responded to the change. At the new base-case spot rate, no-hedge proceeds became **$5,610,000.00**, while forward proceeds remained fixed at **$5,969,550.00**, as expected. The put's downside-floor behavior was also preserved.

**What I did:** I restored `S0_in` to **1.1144** after the test and checked the results again. This was a formula spot-check, not a claim that I manually reviewed every calculated cell.

## Finding 3 — Money-market calculations and forward parity

**What I checked:** I reviewed the three money-market steps: borrowing EUR today, converting to USD, and investing USD until settlement. I also checked the implied forward rate and parity status.

**What I found:** The workbook showed approximately **€5,023,455.38** borrowed, **$5,598,138.67** converted, and **$5,716,722.80** at settlement. The implied forward rate was **1.120926**, compared with the assigned forward placeholder of **1.1705**. The workbook correctly reported **“FLAG: mismatch.”**

**What I did:** I kept the original assigned inputs instead of changing them to make the parity check pass. The mismatch is expected under my current Stage 2 assumptions and needs to remain visible.

## Finding 4 — Scenario-index number formatting

**What I checked:** I looked at the first index in the `Sensitivity` worksheet.

**What I found:** The first scenario showed a dash even though Excel's formula bar showed that the underlying value was **0**. The issue was the cell's number format, not the value or formula.

**What I did:** I tested a number-format correction in Excel. I then added a validation rule in specification Version 0.6 requiring indices **0–10** to display as integers, including zero. I verified that the regenerated workbook shows **0** correctly.

## Finding 5 — Incorrect EUR currency symbols

**What I checked:** I reviewed the units in the `Money Market` worksheet.

**What I found:** The first generated workbook displayed the EUR borrowing and repayment amounts in `B10` and `B22` with **$** signs instead of **€** signs. The amounts themselves were correct, but the displayed currency was misleading.

**What I did:** I first confirmed the formatting fix in Excel, then updated specification Version 0.6 to require € for EUR amounts, $ for USD amounts, and no currency symbol for exchange rates. The regenerated workbook correctly displays **€5,023,455.38** and **€5,100,000.00** in the two cells.

## Finding 6 — Settlement date on the Cover sheet

**What I checked:** I compared the Cover sheet's settlement date with the date stated in Section 1 of my specification.

**What I found:** An earlier generated workbook showed **November 14, 2026** instead of the specified settlement date, **May 7, 2027**. The 182-day calculation assumption was not changed.

**What I did:** I corrected this issue in the regenerated build and verified that the final Cover sheet shows **May 7, 2027**. The correct date was already written in the Stage 2 specification.

## Conclusion

The audit identified formatting and display problems that were not obvious from the calculated results alone. I updated the specification to make the currency and scenario-index rules explicit, regenerated the workbook, and checked those corrections in Excel. I also confirmed that the formulas I spot-checked recalculate, the money-market steps match the expected check figures, and the forward-parity mismatch remains flagged rather than being hidden. The final workbook should be committed together with this audit note and the updated AI prompt log.
