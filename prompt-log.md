
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
