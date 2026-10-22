---
title: "TASK-009: AI Clause Assistant & Intelligent Parameterization Engine"
date: "2026-10-22"
type: task
status: todo
priority: P1
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-011, TM-012, TM-020]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-005, T-007, T-017]]"
tags:
  - task
  - ai-bot
  - clauses
  - schedule-a
  - nlp
  - poc
---

# TASK-009: AI Clause Assistant & Intelligent Parameterization Engine

> **Status:** `todo` | **Priority:** `P1` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-011`, `TM-012`, `TM-020` | Decisions `Q09`, `Q13`, `Q14`, `Q26` | QA Cases `T-005`, `T-007`, `T-017` | Meetings [[retracct/meetings/2026-10-15-ai-bots-module|2026-10-15 AI Bots Module]], [[retracct/meetings/2026-10-12-counter-offers-email-ingestion|2026-10-12 Counter-Offers Discovery]]

---

## 1. What This Feature Does (In Plain English)

When drafting an offer, real estate agents must insert legal clauses into Schedule A (e.g. protecting the buyer if they cannot get a mortgage or if the home inspection reveals major defects).

In Ontario real estate practice, agents have used the same **10 standard clauses** from the official OREA clause booklet for over 30 years.

This task creates the **AI Clause Assistant & Parameter Bot**:
1. **Interactive Clause Assistant:** Instead of hunting through PDF booklets or typing out long legal paragraphs, the agent simply clicks or types: *"Mortgage condition for 5 days"*.
2. **Conversational Prompts:** The bot asks simple, targeted questions to fill in the blanks:
   > *"Mortgage condition for 5 business days? What is the maximum interest rate (e.g. 4.2%) and loan amount?"*
3. **Office vs. Personal Clauses:**
   * **Office Clauses:** Master templates approved by the Broker of Record for the entire brokerage.
   * **Personal Clauses:** Customized clauses saved by an individual agent for frequent reuse.
4. **Mandatory Human Approval Gate (`Q14`):** The AI **never** secretly inserts a clause into a legal contract. It displays the drafted clause in a review box, and the agent must click **"Approve & Insert into Schedule A"**.
5. **AI Counter-Offer Summarizer:** When the other agent sends back a counteroffer, the bot automatically compares the two documents and gives a 3-bullet summary: *"Price increased by $95,000, closing date unchanged, seller changed expiry to tomorrow 8:00 PM"*.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Open Clause Drawer:** In the Schedule A section of an offer, the agent clicks **"Clause Assistant"**.
2. **Select Standard Clause or Type Intent:**
   * **Option A:** Select from the 10 Standard Clauses (Mortgage, Inspection, Lawyer Review, Property Access, Insurance, etc.).
   * **Option B:** Type plain English: *"Buyer wants 2 visits before closing to measure rooms for furniture"*.
3. **Bot Prompts Variables:**
   * The assistant asks: *"Pre-closing access condition: How many visits (default: 2)? How many hours each visit (default: 1 hour)?"*
4. **Instant Clause Preview:**
   * The bot generates the exact standard legal clause:
     > *"The Buyer shall have the right to visit the property 2 times prior to closing, for a period not exceeding 1 hour each visit, upon giving 24 hours written notice..."*
5. **Click to Insert:** The agent reviews the text and clicks **"Approve & Add to Offer"**. The clause is safely added to Schedule A.

---

## 3. Real Estate & Legal Rules (Plain English)

* **Human-in-the-Loop (`Q14`):** In real estate law, software cannot autonomously bind clients to contractual terms. Every AI-suggested clause requires explicit human agent approval.
* **Client Privacy Firewall (`Q26`, `TM-020`):** Client private data (such as driver's licenses, SIN numbers, or personal banking info) must **never** be sent to external AI services. Only sanitized clause parameters (e.g. dates, interest rates, visit counts) are processed.
* **10 Time-Tested Categories:** Seeds the baseline library with standard clauses: New Mortgage, Mortgage Assumption, Inspection, Lawyer Review, Access Visits, Insurance, Deposit Balance, Deposit Increase, Assignment, and Status Certificate.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] In Schedule A, open the Clause Assistant and type *"Need 5 days financing at 4.2% rate for 600k"*.
- [ ] Verify the bot asks any missing questions and generates the Mortgage Financing clause with 5 days, 4.2%, and $600,000 properly filled in.
- [ ] Confirm the clause does not appear in Schedule A until you explicitly click "Approve & Add to Offer".
- [ ] In an incoming counteroffer, click "Summarize Differences" and confirm the bot displays the price change and date changes clearly.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface ClauseDefinition {
  clauseId: string;
  category: "MORTGAGE" | "INSPECTION" | "LAWYER_REVIEW" | "PROPERTY_ACCESS" | "INSURANCE";
  libraryScope: "STANDARD_OREA" | "OFFICE" | "PERSONAL";
  title: string;
  templateText: string;
  variableDefinitions: Array<{
    key: string;
    label: string;
    type: "INTEGER_DAYS" | "PERCENTAGE" | "CURRENCY";
    promptQuestion: string;
  }>;
}

export interface ClausePromptSession {
  sessionId: string;
  agentIntentInput: string;
  extractedParameters: Record<string, any>;
  generatedClauseText: string;
  approvalStatus: "PENDING_AGENT_APPROVAL" | "APPROVED";
}
```

### Endpoints to Implement
* `POST /api/v1/clauses/prompt-session` — NLP parameter extractor returning structured proposal.
* `GET /api/v1/offers/{id}/diff` — Compares current offer against parent version and outputs bulleted change summary.

### Developer Checklist
- [ ] Seed database with the 10 standard OREA clauses and mustache variable definitions.
- [ ] Sanitize prompt payloads before external API calls to block client PII (satisfies QA test `T-017`).
- [ ] Require explicit human approval event (`AI_CLAUSE_APPROVED`) before appending to Schedule A (satisfies QA test `T-007`).

</details>
