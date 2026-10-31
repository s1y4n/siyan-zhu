---
type: brief
title: "FX Hedging Executive Brief — Lab Instruments"
scenario: lab-instruments
engagement: fx-hedging
capability: fx-hedging
date: 2026-10-30
status: committed
hypothesis: "I expect the forward contract to be the most practical hedge because it locks in $5,969,550 without an upfront option premium or additional borrowing."
llm_used: "ChatGPT (OpenAI GPT-6) — drafting and editing support"
---

# FX Hedging Executive Brief — Lab Instruments

**To:** Chief Financial Officer  
**From:** Siyan Zhu  
**Date:** October 30, 2026  
**Scenario:** 9 — lab-instruments  
**LLM Used:** ChatGPT (OpenAI GPT-6), for drafting and editing support

## Executive Summary — Foreign Exchange Exposure

Our company, a U.S. manufacturer of laboratory mass spectrometers, is expecting a payment of EUR 5,100,000 from an Austrian research institute in 182 days. Since the payment will be in euros but we need U.S. dollars, we face foreign exchange risk: the euro amount is fixed, but the dollar amount isn't. If the euro weakens before payment, we could receive less than expected. For example, if the exchange rate drops from a hypothetical 1.20 to 1.10 USD per euro, our dollar receipts would decrease from $6,120,000 to $5,610,000. This 8.33% decline would reduce our proceeds by $510,000, affecting our budget and cash flow.

## Methods — Hedging Alternatives

**Forward Contract:** One possible strategy is a forward contract. The indicative rate is EURUSD 1.1705 for 182 days, meaning we could agree today to sell our euros at that rate when payment arrives. This would lock in $5,969,550 and provide certainty about our cash flow. However, we would miss out on potential gains if the euro strengthens.

**Money-Market Hedge:** Another strategy is to borrow euros today, convert them into dollars at the current spot rate, and invest the dollars until payment arrives. We would borrow an amount that grows to EUR 5,100,000 with interest, then use the customer's payment to repay the loan. Like a forward contract, this provides a predictable dollar amount and can be useful if forwards are unavailable or mispriced. However, it requires borrowing capacity and involves more steps.

**Put Option:** The third strategy is buying a EUR put option, which gives us the right, but not the obligation, to sell euros at a set exchange rate. Our strike price is the current spot rate. This protects us if the euro weakens while still allowing us to benefit if it strengthens. The disadvantage is the upfront premium, which we pay whether or not we use the option. At $0.0194 per euro, the total premium would be $98,940.

## Limitations & Next Steps

**Assumptions:** We assume the Austrian customer pays the full EUR 5.1 million on time and that we will convert the entire payment into dollars. We also assume the quoted forward rate and option premium will be available when we enter the contracts, and that we have access to the borrowing and investment rates needed for a money-market hedge. We would verify these quotes and rates before acting.

**Hypothesis:** I expect the forward contract to be the most practical strategy because it locks in $5,969,550 without an upfront option premium or additional borrowing. It would provide predictable cash flow while keeping the process relatively simple.

**What Would Prove the Hypothesis Wrong:** My hypothesis could be proven wrong if the money-market hedge guarantees more than $5,969,550 after costs, or if the put option produces better net proceeds under reasonable exchange-rate scenarios.

**Analysis Plan:** In Stage 2, we will design the workbook's tabs, formulas, and validation checks. In Stage 3, we will use AI to build the workbook, then audit and correct it. Stage 4 will replace placeholder values with sourced, timestamped market data. Finally, in Stage 5, we will verify the results through independent AI analysis and manual calculations before presenting our recommendation to the CFO.

## References

- Stauffer, A. (2026, October 18). *Scenario 9: lab-instruments* [Course assignment email].
- Stauffer, A. (2026). *Stage 1 — Executive Brief*. https://adamwstauffer.github.io/ai-lms/fx-hedging-stage1.html
