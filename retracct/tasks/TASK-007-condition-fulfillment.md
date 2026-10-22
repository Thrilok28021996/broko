---
title: "TASK-007: Condition Waiver / Fulfillment & Countdown"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-013]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-008]]"
tags:
  - task
  - conditions
  - countdown
  - orea-123
  - orea-124
  - poc
---

# TASK-007: Condition Waiver / Fulfillment & Countdown

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-013` | Decisions `Q18`, `Q19` | QA Cases `T-008` | Meetings [[retracct/meetings/2026-10-15-ai-bots-module|2026-10-15 AI Bots Module]]

---

## 1. What This Feature Does (In Plain English)

Most residential purchases in Ontario are **Conditional Deals** (e.g., *"This deal is conditional for 5 business days upon the buyer arranging a satisfactory mortgage, and 5 business days for a home inspection"*).

This task creates the **Condition Countdown & Resolution Engine**:
1. **Live Countdown Rings:** The deal dashboard displays a visual countdown clock for each condition so the agent never misses a deadline.
2. **Ontario Holiday & Business-Day Smart:** The system knows Ontario statutory holidays (Family Day, Victoria Day, Thanksgiving, etc.) and weekends. If a condition says "5 business days", it automatically skips Saturdays, Sundays, and holidays.
3. **Fulfilling Conditions:** When the bank approves the loan, the agent uploads or signs the official **Notice of Fulfillment (OREA Form 124)**.
4. **Waiving Conditions:** If the buyer decides to skip the inspection, the agent uploads the signed **Waiver (OREA Form 123)**.
5. **Automatic Firm Deal:** The instant the last condition is marked fulfilled or waived, the entire transaction automatically flips to **FIRM** (a completed, binding sale!).
6. **No Silent Cancellations:** If a deadline expires without paperwork, the system raises an alert; it **never** unilaterally declares the deal dead without human review.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Conditional Dashboard View:**
   * **Condition 1:** 🏦 *Mortgage Financing Approval* — 🟢 3 days 14 hours remaining.
   * **Condition 2:** 🔍 *Home Inspection* — 🟢 3 days 14 hours remaining.
2. **Financing Gets Approved:**
   * The buyer's bank sends the mortgage commitment.
   * The agent clicks **"Fulfill Condition"** next to Mortgage Financing.
   * The agent uploads the signed OREA Form 124 (Notice of Fulfillment).
   * Status turns to 🟢 **Fulfilled**.
3. **Inspection Passes:**
   * The home inspection report comes back clean.
   * The agent clicks **"Waive Condition"** next to Home Inspection and attaches signed Form 123.
   * Status turns to 🟢 **Waived**.
4. **Deal Goes Firm:**
   * A celebration banner appears: 🎉 **Transaction is now FIRM!**
   * The system automatically prepares the file for conveyancing and closing.

---

## 3. Real Estate & Legal Rules (Plain English)

* **Exact Ontario Timezone:** All condition expiries calculate strictly in Ontario time (`America/Toronto`), accounting for Daylight Saving Time changes.
* **Proper Forms Required:**
  * To *fulfill* a condition: Use OREA Form 124 (Notice of Fulfillment).
  * To *waive* a condition: Use OREA Form 123 (Waiver).
  * To *extend* a deadline: Use OREA Form 120 (Amendment).
* **No Autonomous Forfeiture (`Q19`):** If a deadline passes, the status changes to `EXPIRED_UNRESOLVED` and notifies the agent. The software must never make an autonomous legal determination to cancel a deal.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] On a conditional deal with a 5-day condition created on a Thursday, verify the countdown skips Saturday and Sunday.
- [ ] Click "Fulfill Condition", attach a test Notice of Fulfillment (Form 124), and verify the badge turns green (*Fulfilled*).
- [ ] Waive the remaining condition and verify the transaction banner immediately changes from *Conditional* to *FIRM*.
- [ ] In a test case where deadline has expired, confirm status shows *Expired / Unresolved* with an urgent action banner.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface ConditionResolutionPayload {
  transactionId: string;
  conditionId: string;
  resolutionType: "FULFILLMENT" | "WAIVER" | "AMENDMENT";
  documentDetails: {
    formUsed: "OREA_FORM_124_NOTICE_OF_FULFILLMENT" | "OREA_FORM_123_WAIVER" | "OREA_FORM_120_AMENDMENT";
    documentId: string;
    signedAt: string;
  };
}
```

### Endpoints to Implement
* `POST /api/v1/conditions/{id}/resolve` — Handles fulfillment/waiver and triggers firm deal state machine evaluator.
* Automated deadline scheduler evaluating reminders at 72h, 24h, and 4h prior to expiry.

### Developer Checklist
- [ ] Build `OntarioCalendarService` with cached Ontario statutory holidays (Family Day, Victoria Day, Canada Day, etc.).
- [ ] Trigger automatic transition to `FIRM` when `COUNT(activeConditions) === 0`.
- [ ] Mark missed deadlines as `EXPIRED_UNRESOLVED` without terminating deal (satisfies QA test `T-008`).

</details>
