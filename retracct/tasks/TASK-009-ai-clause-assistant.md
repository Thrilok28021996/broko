---
title: "TASK-009: AI Clause Assistant & Intelligent Parameterization Engine"
date: "2026-10-22"
type: task
status: todo
priority: P1
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-012, TM-020]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-007, T-017]]"
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

## 1. Description & User Story

As an agent drafting an offer or reviewing an incoming counteroffer, I want an intelligent conversational bot assistant that:
1. Prompts me for key variables (business days, interest rate caps, loan amounts, inspection scope) and populates standard OREA clause booklet templates or synthesizes custom clauses with a mandatory human approval review before inserting into Schedule A.
2. Automatically parses incoming counteroffer documents and generates an executive difference summary (e.g. price change, irrevocability change, condition changes) so I can immediately advise my client.

---

## 2. Technical Data Contract & Schema

```typescript
export interface ClauseDefinition {
  clauseId: string;
  category: "MORTGAGE" | "INSPECTION" | "LAWYER_REVIEW" | "PROPERTY_ACCESS" | "INSURANCE" | "DEPOSIT_BALANCE" | "ASSIGNMENT" | "CUSTOM";
  libraryScope: "OFFICE" | "PERSONAL" | "STANDARD_OREA";
  title: string; // e.g. "Buyer Arranging New First Mortgage"
  templateText: string; // Template with mustache variables, e.g. "This Offer is conditional upon the Buyer arranging a new first mortgage of not less than ${{principalAmount}}..."
  variableDefinitions: Array<{
    key: string;
    label: string;
    type: "INTEGER_DAYS" | "CURRENCY" | "PERCENTAGE" | "DATE" | "STRING";
    defaultValue?: any;
    promptQuestion: string; // e.g. "How many business days do you need for mortgage financing?"
  }>;
}

export interface ClausePromptSession {
  sessionId: string;
  transactionId: string;
  agentIntentInput: string; // e.g. "Need mortgage condition 5 days at 4.2% rate for 600k"
  detectedClauseId?: string;
  extractedParameters: Record<string, any>; // { durationBusinessDays: 5, interestRateCapPercent: 4.2, principalAmount: 600000 }
  generatedClauseText: string;
  confidenceScore: number;
  approvalStatus: "PENDING_AGENT_APPROVAL" | "APPROVED" | "REJECTED" | "MODIFIED";
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Standard 10-Clause Baseline Seed:**
  - Seeds the system with the 10 time-tested standard OREA clause definitions:
    1. *New First Mortgage* (days, principal amount, max interest rate %, amortization).
    2. *Assumption of Existing Mortgage* (interest rate, lender approval days).
    3. *Home Inspection* (licensed inspector, repair threshold, business days).
    4. *Lawyer Review of APS* (business days).
    5. *Pre-Closing Property Access* (number of visits, notice required, hour duration).
    6. *Property Insurance Approval* (business days).
    7. *Deposit Balance of Purchase Price* (due date, certified funds).
    8. *Deposit Increase Installment* (staged payment dates).
    9. *Assignment Right* (builder/seller consent terms).
    10. *Condo Status Certificate Review* (days to receive, days to review).
- [ ] **Conversational Parameterization Bot Flow:**
  - When the agent clicks or mentions "Mortgage Condition", the assistant asks targeted questions:
    - *"Mortgage condition for 5 business days? Anything else you want to mention (e.g., maximum interest rate or principal)?"*
  - Automatically maps answers to typed variables and renders the exact clause text with no blank placeholders.
- [ ] **Office vs. Personal Library Scoping (`Q13`):**
  - **Office Library:** Brokerage-curated templates visible to all brokerage agents, editable only by Broker of Record / Admins.
  - **Personal Library:** Agent-saved clause snippets with custom default parameters.
- [ ] **Mandatory Human-in-the-Loop Review (`Q14`, `T-007`):**
  - AI proposals are displayed in an **Approval Staging Modal**.
  - The system **NEVER** silently inserts an AI-generated or custom clause into Schedule A without explicit agent confirmation.
- [ ] **AI Counteroffer Difference Summarizer:**
  - Compares previous offer version against incoming counteroffer PDF/version.
  - Generates clear bulleted difference list:
    - *Purchase Price:* e.g. `$1,150,000` $\rightarrow$ `$1,245,000` (+ $95,000).
    - *Irrevocable Expiry:* e.g. `2026-10-19 23:59` $\rightarrow$ `2026-10-21 23:59` (+ 2 days).
    - *Schedule A Conditions:* Identifies any added, struck-out, or modified clauses.
  - Presents 3 immediate action triggers: `[Accept Counter]`, `[Counter Round N+1]`, `[Reject / Terminate]`.
- [ ] **Data Privacy & No Unapproved Model Egress (`Q26`, `TM-020`, `T-017`):**
  - Ensures private client identity documents (e.g. driver's licenses, SIN numbers, bank account numbers) are never piped to external AI models.
  - Clause intent prompts only pass sanitized parameters (dates, amounts, property address).
- [ ] **Audit Trail Events:**
  - `AI_CLAUSE_PROMPTED`
  - `AI_CLAUSE_PARAMETERS_EXTRACTED`
  - `AI_CLAUSE_APPROVED_BY_AGENT`

---

## 4. Implementation Steps

1. **Clause Registry Service:**
   - Store baseline OREA clauses in database table `clause_definitions` with JSON variable schema.
   - Implement CRUD endpoints for Personal vs Office clauses.
2. **Interactive NLP / Parameter Extractor:**
   - Implement `ClauseAssistantService.parseIntent(input: string)` using structured tool calling / schema extraction.
   - Match input against standard templates; if no exact template matches, generate draft with explicit disclaimer.
3. **UI Chat / Assistant Drawer:**
   - Collapsible drawer inside Form 100 Schedule A editor:
     - Suggested standard clauses list.
     - Natural language input bar: *"Type condition or select template..."*
     - Parameter prompt bubbles.
     - "Review & Insert to Schedule A" confirmation button.
4. **Guardrail Enforcer:**
   - Sanitize prompt payloads before calling AI service; block any payloads containing PII.

---

## 5. Verification & QA Test Links

- [ ] `T-007`: AI clause drafting approval test — verify custom drafted clause is flagged for agent review and cannot be silently inserted.
- [ ] `T-017`: Privacy guard test — verify that client identity documents and sensitive PII are blocked from external AI model queries.
