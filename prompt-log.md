
# AI Prompt Log

## October 9, 2026 — GitHub Portfolio Setup

**AI Tool:** ChatGPT (OpenAI)

**Task:** Set up my GitHub portfolio repository for Stage 0.

**What I asked AI to do:**
- Explain how to use GitHub Desktop on my MacBook.
- Guide me through creating a public repository.
- Help organize the required portfolio folders.
- Generate starter files for AGENTS.md, CLAUDE.md, and .gitignore.
- Explain how to commit and push files to GitHub.
- Review my personal bio for grammar and clarity.
- Help format my resume in Markdown.

**What I checked or changed:**
- Verified that my repository was public.
- Checked that the required folders and files were created.
- Reviewed my personal bio and resume.
- Reviewed the generated AI conventions and exclusion rules.

**Errors or limitations:**
- AI could not directly edit or publish my local GitHub repository.
- I completed the setup steps myself using VS Code and GitHub Desktop.

**Result:**
Created the initial portfolio structure for future projects.

## October 30, 2026 — FX Hedging Stage 1 Executive Brief

**AI Tool:** ChatGPT (OpenAI)

**Task:** Complete Stage 1 of the FX Hedging project using Scenario 9 (lab-instruments).

**What I asked AI to do:**
- Explain the Stage 1 requirements and my assigned scenario.
- Help me understand the three hedging strategies: forward contract, money-market hedge, and put option.
- Check my calculations and provide feedback on my writing.
- Correct grammar, remove repetition, and organize my original draft while keeping my writing style.
- Help prepare the final Markdown file and guide me through uploading it to GitHub.
- Help correct the missing folder and resume dates from Stage 0.

**What I checked or changed:**
- Reviewed and revised the suggested wording to keep my writing style.
- Checked the scenario information and financial calculations.
- Reviewed the final brief and confirmed the required sections were included.
- Verified that the files were uploaded to the correct GitHub folders.

**Errors or limitations:**
- AI initially used the October 23 deadline from my professor's email, but Lamakū showed October 30 as the actual assignment deadline.
- The exchange rates used to illustrate potential losses were hypothetical, not actual market rates.
- The final hedging recommendation still requires market data and model validation in later stages.

**Result:** Completed the Stage 1 Executive Brief and committed the Markdown file to my GitHub portfolio

## Stage 2 — FX Hedging Model Specification Review

**Date:** November 7, 2026  
**AI Tool:** ChatGPT  
**Purpose:** Develop, review, and improve the Stage 2 technical specification before the Stage 3 workbook build.

### Prompt 1 — Review and Organization

**My actual prompt:**

"This is everything. Make sure to order all of them correctly first."

**AI Feedback:**

ChatGPT identified duplicated section numbers and overlapping sections in my specification. The original template contained ten sections, while the current Stage 2 assignment required eight sections.

**Revision Made:**

I reorganized the specification into the eight required sections, moved the Sensitivity Plan to Section 6, incorporated the auditability checklist into Section 7 (Validation Rules), and moved Outputs to Section 8.

### Prompt 2 — Evaluating AI Suggestions

**My actual prompt:**

"No, walk me through everything to fix it by myself."

**Decision and Reasoning:**

ChatGPT initially suggested replacing my specification with an automatically reorganized file. I rejected this approach because I wanted to understand the changes and make them myself. Instead, I accepted the recommended section order and manually revised the document in GitHub.

### Additional Technical Review — Covered Interest Rate Parity

**Issue Identified:**

ChatGPT identified an inconsistency between the scenario-assigned forward rate of 1.1705 USD/EUR and the implied forward rate calculated using my spot and interest-rate placeholders.

**Before Revision:**

The original template required the money-market hedge and forward hedge to agree within 0.05%, without addressing the possibility of inconsistent placeholder inputs.

**After Revision:**

I updated Sections 5 and 7 to calculate the implied forward rate (`F_IMPLIED`), compare it with the assigned forward rate (`F0_in`), and flag discrepancies exceeding the 0.05% tolerance. I also specified that consistent market quotations would be reviewed in Stage 4.

**Decision and Reasoning:**

I accepted this correction because a failed parity check may result from inconsistent market inputs rather than incorrect formulas. Documenting the discrepancy improves the model's transparency and auditability.

### Reflection

I used ChatGPT throughout the drafting and review process to understand financial concepts, develop formulas, improve the document's organization, and identify inconsistencies. Rather than automatically accepting every suggestion, I reviewed the feedback and made revisions manually in GitHub. This helped me better understand the model's calculations and create a clearer specification for the Stage 3 AI-assisted workbook build.

## November 14, 2026 — FX Hedging Stage 3: AI-Assisted Build and Audit

**AI Tool:** ChatGPT (OpenAI)

**Task:** Build and audit an Excel workbook for FX Hedging Scenario 9 using my Stage 2 specification.

**What I asked AI to do:**
- I shared my Stage 2 specification and the Stage 3 assignment instructions.
- I asked ChatGPT to walk me through the assignment and generate a downloadable Excel workbook.
- I used ChatGPT to help me understand how to check named ranges, formulas, and sensitivity calculations in Excel.
- I asked for help correcting formatting errors and regenerating the workbook.

**Some prompts I used:**
- "this is my stage 3. walk me through it."
- "okay lets start."
- "it works let move on to the currency fix"
- "are u sure we can regenerate the workbook so fast? did we finish all the professors requirements already?"

**What I checked or changed:**
- Verified all 11 named input ranges in Excel's Name Manager.
- Checked that calculated cells contained formulas instead of hard-coded values.
- Changed the spot rate from 1.1144 to 1.1000 to test whether the sensitivity table and chart recalculated.
- Verified the money-market hedge calculations and expected parity mismatch.
- Found that the first sensitivity scenario displayed a dash instead of zero.
- Found that two EUR amounts incorrectly displayed dollar signs.
- Updated my Stage 2 specification to Version 0.6 with clearer formatting requirements.
- Regenerated the workbook and verified the corrections.
- Corrected an incorrect settlement date on the Cover sheet.

**Errors or limitations:**

The first AI-generated workbook had formatting problems that I found while reviewing it in Excel. I also noticed that an earlier workbook showed the wrong settlement date. I did not assume the AI output was correct and checked the workbook against my specification.

ChatGPT also had trouble providing a working download link, so I requested another downloadable copy.

**Result:**

Created an Excel workbook with nine worksheets, 11 named inputs, formula-driven calculations, and a sensitivity chart. I reviewed the workbook in Excel, documented my findings in a separate audit report, and verified the regenerated corrections. I used ChatGPT to help build and troubleshoot the model, but I performed the Excel checks and GitHub uploads myself.

2026-11-20 — Stage 3: Professor Feedback and Formula Audit

AI Tool: ChatGPT (OpenAI)

Prompt: I provided ChatGPT with my professor's Stage 3 feedback and asked for help improving my formulas-only audit to check all 251 calculated cells instead of just one.

AI Assistance: ChatGPT helped create a workbook-wide formula integrity check using Excel's ISFORMULA() and SUMPRODUCT() functions. It added a Formula Audit worksheet and a 15th validation check on the Cover. ChatGPT also helped document the results as Finding 7 in my build-audit analysis.

Results and Verification: The audit confirmed that all 251 original calculated cells contained formulas, with no missing or overwritten formulas. I opened the updated workbook in Excel and personally confirmed that the Formula Audit and Cover checks both displayed PASS. The original model inputs and calculation formulas were preserved.

Human Review: I reviewed the changes and verified the results in Excel before preparing the updated files for GitHub.

### 2026-11-24 — Stage 4: Market Data and Workbook Population

**AI Tool:** ChatGPT (OpenAI)

**Prompt:** I asked ChatGPT to help me complete Stage 4 using my audited Stage 3 FX hedging workbook, including finding market data, updating the inputs, checking calculations, and documenting the results.

**AI Assistance:** ChatGPT helped locate market data from the European Central Bank, U.S. Treasury, and Bank of Finland. It helped populate my Excel workbook with the updated spot and interest rates, calculate the CIP-implied forward, and update the validation checks to work with the new inputs. ChatGPT also helped prepare the market-data memo, organize the sources and assumptions, and analyze differences between my workbook and the FX Hedging Lab.

**Results and Verification:** I opened the updated workbook in Excel and confirmed that the formula audit passed with all 257 calculated cells containing formulas. I also entered the inputs into the professor's FX Hedging Lab and compared the forward, money-market, and sensitivity results with my workbook. The calculations matched, except for a difference in the put-option results caused by the treatment of the upfront premium. The workbook includes the premium's future value, while the lab uses the upfront amount. I kept the workbook's original treatment because it was approved in Stage 3 and documented the difference in the memo.

**Human Review:** I personally verified the Excel checks and FX Lab results. The original scenario assumptions, including the assigned forward quote and option premiums, were preserved. The forward-parity discrepancy was documented rather than adjusted to force a passing result.
