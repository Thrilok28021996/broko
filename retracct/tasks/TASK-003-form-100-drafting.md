---
title: "TASK-003: Form 100 Offer Drafting & Schedule A Conditions"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-003, TM-005, TM-010, TM-012, TM-013]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-003, T-007, T-008]]"
tags:
  - task
  - drafting
  - orea-form-100
  - schedule-a
  - clauses
  - poc
---

# TASK-003: Form 100 Offer Drafting & Schedule A Conditions

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-003`, `TM-005`, `TM-010`, `TM-012`, `TM-013` | Decisions `Q08`, `Q09`, `Q11`, `Q12`, `Q13`, `Q14`, `Q18`, `Q19` | QA Cases `T-003`, `T-005`, `T-007`, `T-008` | Meetings [[retracct/meetings/2026-10-15-ai-bots-module|2026-10-15 AI Bots Module]], [[retracct/meetings/2026-10-12-counter-offers-email-ingestion|2026-10-12 Counter-Offers Discovery]]

---

## 1. Technical Objective & User Context

### Technical Objective (What Developers Build)
Build the guided authoring engine for the Ontario Agreement of Purchase and Sale (OREA Form 100), financial reconciliation calculator (`Price = Deposit + Balance`), Schedule A condition compiler, Land Registry business-day date calculators, and counteroffer versioning session tracking with strike-outs and Confirmation of Acceptance (COA).

### End-User Persona
* **Primary User:** Licensed Real Estate Agent (Buyer Representative)
* **Counterparty User:** Listing Agent (receiving and countering offers)

### Business Rationale & Domain Context
OREA Form 100 is the core legal contract for Ontario residential sales. Under RECO regulations, counteroffers must preserve document continuity across negotiation rounds, tracking sequential price strike-outs, initials, and flips of irrevocability until the final accepting party executes the Confirmation of Acceptance (COA).

---

## 2. Technical Data Contract & Schema

```typescript
export interface Form100OfferPayload {
  transactionId: string;
  offerVersionId: string; // e.g. "OFF-001-v1"
  buyerNames: string[];
  sellerNames: string[];
  propertyDetails: {
    address: string;
    frontageFeet?: number;
    depthFeet?: number;
    legalDescription: string;
  };
  financialTerms: {
    purchasePrice: number; // e.g. 1,250,000.00
    depositAmount: number; // e.g. 50,000.00
    depositPayableMethod: "HEREWITH" | "UPON_ACCEPTANCE" | "AS_OTHERWISE_DESCRIBED";
    additionalDepositAmount?: number;
    balanceOfPurchasePrice: number; // purchasePrice - totalDeposits
  };
  scheduleTerms: {
    irrevocableDeadline: string; // ISO 8601 with America/Toronto TZ (e.g. 2026-10-25T23:59:00-04:00)
    completionDate: string;      // Closing Date (YYYY-MM-DD, must be a Land Registry business day)
    requisitionDate: string;     // Title Search deadline (YYYY-MM-DD, typically 14-30 days before closing)
    chattelsIncluded: string[];  // e.g. ["Stainless Steel Fridge", "Gas Stove", "Washer/Dryer"]
    fixturesExcluded: string[];  // e.g. ["Dining Room Chandelier"]
    rentalItems: string[];       // e.g. ["Hot Water Tank (Reliance $34.99/mo)"]
  };
  scheduleAConditions: Array<{
    conditionId: string;
    clauseType: "MORTGAGE" | "INSPECTION" | "LAWYER_REVIEW" | "PROPERTY_ACCESS" | "INSURANCE" | "CUSTOM";
    sourceLibrary: "OFFICE" | "PERSONAL" | "STANDARD_OREA";
    rawClauseText: string;
    parameters: {
      durationBusinessDays?: number;
      interestRateCapPercent?: number; // e.g. 4.25
      principalAmount?: number;
      amortizationYears?: number;
      numberOfAccessVisits?: number;
      accessDurationHours?: number;
    };
    fulfillmentParty: "BUYER" | "SELLER";
    benefitingParty: "BUYER" | "SELLER" | "MUTUAL";
    irrevocableExpiry: string; // Computed ISO timestamp
  }>;
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Form 100 Guided Builder:**
  - Validates that `purchasePrice = depositAmount + additionalDepositAmount + balanceOfPurchasePrice`.
  - Rejects offers where `irrevocableDeadline` is set in the past.
  - Automatically calculates default `requisitionDate` (Title Search) as 14 to 30 days prior to `completionDate`.
  - Enforces that `completionDate` does not fall on an Ontario statutory holiday or weekend.
- [ ] **Schedule A Clause Compilation:**
  - Provides instant insertion of standard OREA booklet conditions:
    1. *Mortgage Financing:* Populates loan principal, interest cap (e.g. 4.0% per annum), amortization, and 5 business-day waiver window.
    2. *Property Inspection:* Qualified home inspector clause with repair-cost threshold and 5 business-day window.
    3. *Lawyer Review:* Condition for buyer's lawyer to review agreement within 3–5 business days.
    4. *Property Access:* Pre-closing buyer access (e.g., 2 visits of 1-hour duration prior to closing).
    5. *Insurance:* Satisfactory property insurance condition.
- [ ] **Library Segmentation (`Q13`):**
  - Distinguishes between **Office Clauses** (brokerage approved) and **Personal Clauses** (agent saved).
- [ ] **Clause Conflict & Variable Checking:**
  - Prevents drafting with unpopulated template variables (e.g. `[insert interest rate]` or `[insert days]`).
  - Warns if multiple contradictory financing conditions are selected.
- [ ] **Warn-Only Form Control (`Q12`, `TM-006`):**
  - If OREA Form 100 version timestamp is unverified, displays non-blocking warning banner: *"Using unverified OREA Form 100 version. Consult Broker of Record prior to firm commitment."*
- [ ] **Audit Trail Events:**
  - `OFFER_DRAFT_CREATED`
  - `SCHEDULE_A_COMPILED`
  - `OFFER_TERMS_VALIDATED`

---

## 4. Implementation Steps

1. **Offer Service Engine:**
   - Create `OfferDraftingService` with methods `createDraft()`, `calculateFinancialBalance()`, `addScheduleAClause()`.
   - Implement date calculation engine for Ontario Land Registry business days.
2. **Standard Clause Booklet Ingestion:**
   - Seed baseline database with the 10 standard OREA conditions and typed variable schemas.
3. **Drafting UI Component:**
   - Step 1: Parties and Legal Property Description.
   - Step 2: Price and Deposit Schedule (dynamic balance recalculator).
   - Step 3: Important Dates (Irrevocable, Title Search, Closing Date).
   - Step 4: Schedule A Builder with Clause Selector (Office vs Personal tabs) and variable inputs.
4. **Validation Pipeline:**
   - Ensure all inputs pass strict schema verification before enabling "Proceed to E-Signature" action.

---

## 5. Verification & QA Test Links

- [ ] `T-003`: Verify drafting can proceed with persistent warning when form version is unverified.
- [ ] `T-007`: Verify AI clause suggestions are flagged for human review before adding to Schedule A.
- [ ] `T-008`: Verify countdown calculation correctly skips Ontario statutory holidays and weekends.
