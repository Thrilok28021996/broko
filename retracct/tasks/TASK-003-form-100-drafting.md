---
title: "TASK-003: Form 100 Offer Drafting & Schedule A Conditions"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-003, TM-005, TM-010, TM-012, TM-013]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-003, T-005, T-007, T-008]]"
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

## 1. What This Feature Does (In Plain English)

This is the heart of the platform: creating the official Ontario legal offer to purchase a residential home (**OREA Form 100 — Agreement of Purchase and Sale**).

It lets the agent easily fill in all the key terms of the deal without making costly calculation or omission mistakes:
1. **Property & Legal Description:** Enters the complete municipal address, side of street, frontage and depth dimensions, and official legal description (Lot, Plan, PIN, Municipality) along with full legal names of Buyer(s) and Seller(s) to satisfy OREA Form 100 requisitions.
2. **Price & Deposit Structure:** Enters the purchase price (in figures and legal words), the initial deposit, any supplementary/additional deposits (e.g. upon condition removal), the deposit payment timing ("Herewith", "Upon Acceptance" [within 24h], or "As otherwise described"), and the deposit holder (held in trust). The system automatically computes the remaining unadjusted balance of the purchase price due on completion (subject to closing adjustments).
3. **Important Dates:** Sets when the offer expires (irrevocable deadline), when the lawyer checks the title (requisition date), and when the buyer gets the keys (closing date).
4. **What Stays with the House:** Checks off what chattels are included (e.g., stainless steel fridge, stove, washer/dryer) and what items the seller is taking.
5. **Schedule A Conditions:** Easily inserts standard Ontario protection clauses (e.g. 5 days to get mortgage financing approved, or 5 days for a home inspection).
6. **Counter-Offers:** When the seller counters back (e.g. asking for $50,000 more), the system tracks the negotiation by showing the old price crossed out with initials on the same document, keeping a clean legal paper trail until the **Confirmation of Acceptance** is signed.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Start Offer:** The agent clicks **"Draft Form 100 Offer"**.
2. **Step 1 — Price & Deposit:**
   * Agent types the purchase price (e.g. `$1,200,000`).
   * Agent enters the initial deposit (e.g. `$50,000`), selects the deposit holder (e.g. Listing Brokerage in trust), and chooses "Payable Upon Acceptance" (within 24 hours).
   * Agent can optionally add a supplementary deposit (e.g. `$25,000` upon waiver of conditions).
   * The screen immediately updates: *"Unadjusted Balance Due on Closing: $1,125,000 (subject to closing adjustments)"*.
3. **Step 2 — Dates & Expiry:**
   * Agent sets the offer expiry time (e.g. 11:59 PM tomorrow).
   * Agent picks the closing date (e.g. December 15). The system automatically warns if the chosen date falls on a weekend or Ontario statutory holiday.
   * The title search date is automatically suggested (typically 14 to 30 days before closing).
4. **Step 3 — Schedule A Conditions:**
   * The agent clicks "+ Add Condition" and selects **"Mortgage Financing"**.
   * A clean box asks: *"How many business days?"* (Default: 5). *"Maximum interest rate?"* (e.g. 4.2%).
   * The legal clause is generated instantly with the numbers filled in.
   * Agent clicks "+ Add Condition" and adds **"Home Inspection"** (5 business days).
5. **Review & Preview:** The agent clicks **"Preview Form 100"** to see the completed, ready-to-sign PDF.

---

## 3. Real Estate & Legal Rules (Plain English)

* **The Math Must Add Up:** Purchase Price must exactly equal Total Deposits (Initial + Additional/Supplementary) + Balance of Purchase Price on Closing. The system guarantees mathematical reconciliation before signing, while explicitly flagging that final closing funds are subject to closing adjustments (taxes, utilities, common expenses) prepared in the statement of adjustments.
* **No Holiday Closings:** Land registry offices in Ontario are closed on weekends and statutory holidays (like Thanksgiving or Family Day). Closing dates must be valid business days.
* **Single-Session Counter-Offers:** When a counter-offer is received, never create a brand-new blank contract. Show the previous price struck through with initials on the existing document chain (Ontario RECO best practice).
* **Confirmation of Acceptance (COA):** The deal is only locked once the last party to accept signs the COA box on Page 5 of Form 100.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] Try picking Sunday as a closing date and verify the system prompts you to choose a business day instead.
- [ ] Start a new Form 100 offer, enter `$1,000,000` price and `$50,000` deposit. Verify the closing balance displays `$950,000`.
- [ ] Add a Mortgage Financing condition for 5 days at 4% rate and check the preview to confirm the clause text is filled out without any blank blanks (`[ ]`).
- [ ] Simulate a counteroffer: change price to `$1,050,000` and confirm the preview displays `$1,000,000` crossed out with an initial box next to it.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface Form100OfferPayload {
  transactionId: string;
  offerVersionId: string; // e.g. "OFF-001-v1"
  propertyDetails: {
    municipalAddress: string;
    sideOfStreet?: "NORTH" | "SOUTH" | "EAST" | "WEST";
    frontageDepth?: string; // e.g. "50.00 ft x 120.00 ft"
    legalDescription: string; // Lot, Plan/Concession, PIN
    municipality: string;
  };
  financialTerms: {
    purchasePrice: number;
    depositAmount: number;
    depositPayableMethod: "HEREWITH" | "UPON_ACCEPTANCE" | "AS_OTHERWISE_DESCRIBED";
    depositHolder: string; // e.g. "Listing Brokerage in Trust"
    additionalDepositAmount?: number;
    balanceOfPurchasePrice: number; // purchasePrice - (depositAmount + (additionalDepositAmount || 0)), subject to adjustments
  };
  scheduleTerms: {
    irrevocableDeadline: string; // ISO 8601 America/Toronto
    completionDate: string;      // YYYY-MM-DD
    requisitionDate: string;     // YYYY-MM-DD
    chattelsIncluded: string[];
    fixturesExcluded: string[];
  };
  scheduleAConditions: Array<{
    conditionId: string;
    clauseType: "MORTGAGE" | "INSPECTION" | "LAWYER_REVIEW" | "PROPERTY_ACCESS";
    rawClauseText: string;
    parameters: {
      durationBusinessDays?: number;
      interestRateCapPercent?: number;
      principalAmount?: number;
    };
    irrevocableExpiry: string;
  }>;
}
```

### Endpoints to Implement
* `POST /api/v1/offers` — Saves offer terms and validates financial balance equation.
* `POST /api/v1/offers/{id}/counter` — Creates new version (`v2`) on the same offer entity with strike-outs and flipped irrevocability.
* `POST /api/v1/offers/{id}/accept-coa` — Records Confirmation of Acceptance timestamp.

### Developer Checklist
- [ ] Validate `purchasePrice === depositAmount + (additionalDepositAmount || 0) + balanceOfPurchasePrice` (return HTTP 422 on mismatch).
- [ ] Ensure `propertyDetails` contains required legal description (Lot/Plan/PIN) and frontage/depth before PDF generation.
- [ ] Integrate Ontario holiday calendar to reject weekend/holiday `completionDate`.
- [ ] Implement PDF stamping of strike-outs and initial boxes on counter-offer rounds.
- [ ] Automated tests passing for QA cases `T-003`, `T-005`, `T-007`, and `T-008`.

</details>
