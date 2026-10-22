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

## 1. Technical Objective & User Context

### Technical Objective (What Developers Build)
Build the condition countdown engine in the Ontario timezone (`America/Toronto`), accounting for statutory holidays and business days, resolution workflows for Notice of Fulfillment (OREA 124) and Waiver (OREA 123) uploads, automatic deal status progression to `FIRM`, and an `EXPIRED_UNRESOLVED` escalation state.

### End-User Persona
* **Primary User:** Licensed Real Estate Agent (Buyer Representative)
* **Supervisory User:** Broker of Record (BOR)

### Business Rationale & Domain Context
In conditional real estate purchases, financing and inspection conditions must be satisfied or waived prior to precise irrevocable deadlines. Under PRD Decision `Q19`, if a condition deadline passes without resolution documents, the software must never unilaterally cancel the agreement; it must flag `EXPIRED_UNRESOLVED` and escalate to human stakeholders (`T-008`).

---

## 2. Technical Data Contract & Schema

```typescript
export interface ConditionResolutionPayload {
  transactionId: string;
  conditionId: string;
  resolutionType: "FULFILLMENT" | "WAIVER" | "AMENDMENT" | "MUTUAL_RELEASE";
  documentDetails: {
    formUsed: "OREA_FORM_124_NOTICE_OF_FULFILLMENT" | "OREA_FORM_123_WAIVER" | "OREA_FORM_120_AMENDMENT" | "OREA_FORM_122_MUTUAL_RELEASE";
    documentId: string; // UUID of executed PDF
    signedAt: string;   // ISO 8601
    deliveredToCounterpartyAt: string; // ISO 8601
  };
  benefitingPartyAcknowledged: boolean;
  notes?: string;
}

export interface ConditionStateRecord {
  conditionId: string;
  title: string; // e.g. "Buyer Financing Approval"
  clauseType: "MORTGAGE" | "INSPECTION" | "LAWYER_REVIEW" | "STATUS_CERTIFICATE";
  irrevocableDeadline: string; // ISO 8601 America/Toronto
  daysRemaining: number;
  hoursRemaining: number;
  status: "ACTIVE" | "EVIDENCE_SUBMITTED" | "FULFILLED" | "WAIVED" | "EXPIRED_UNRESOLVED" | "TERMINATED";
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Exact Ontario Deadline Calculation Engine (`TM-013`, `T-008`):**
  - All condition deadlines calculate in `America/Toronto` timezone.
  - Distinguishes accurately between **Calendar Days** and **Ontario Business Days** (excluding Saturdays, Sundays, and Ontario statutory holidays: Family Day, Good Friday, Victoria Day, Canada Day, Civic Holiday, Labour Day, Thanksgiving, Christmas, Boxing Day).
  - Handles Daylight Saving Time (DST) shifts seamlessly without 1-hour calculation errors (`T-008`).
- [ ] **Structured Resolution Workflows:**
  - **Fulfillment:** Requires executed OREA Form 124 (Notice of Fulfillment) signed by the party whose benefit the condition was inserted for.
  - **Waiver:** Requires executed OREA Form 123 (Waiver) signed by the benefiting party.
  - **Amendment:** If parties extend deadline, links signed OREA Form 120 and recalculates countdown timer.
  - **Mutual Release:** If condition cannot be met, triggers OREA Form 122 workflow and advances transaction to `TERMINATED`.
- [ ] **Automatic Deal State Transition:**
  - When **ALL** active conditions on a transaction reach status `FULFILLED` or `WAIVED`, the transaction automatically transitions from `ACCEPTED_CONDITIONAL` to `FIRM`.
- [ ] **No Silent Missed Deadline Execution (`Q19`):**
  - If a deadline passes without a signed fulfillment or waiver, the condition state changes to `EXPIRED_UNRESOLVED` and generates urgent BOR/agent alerts.
  - The system **NEVER** makes an autonomous legal determination of deal termination without human confirmation.
- [ ] **Audit Trail Events:**
  - `CONDITION_ACTIVATED`
  - `CONDITION_DEADLINE_APPROACHING_ALERT`
  - `CONDITION_FULFILLED`
  - `CONDITION_WAIVED`
  - `TRANSACTION_STATUS_CHANGED_TO_FIRM`

---

## 4. Implementation Steps

1. **Date & Holiday Calculation Utility:**
   - Implement `OntarioCalendarService` with cached statutory holiday table (2026–2035).
   - Methods: `addBusinessDays(startDate, count, cutoffTime)`, `isBusinessDay(date)`.
2. **Condition State Machine Service:**
   - Implement transition matrix (`ACTIVE` → `EVIDENCE_SUBMITTED` → `FULFILLED` / `WAIVED` / `EXPIRED_UNRESOLVED`).
   - Trigger deal firming evaluator when any condition status updates.
3. **UI Real-Time Countdown Cards:**
   - Display circular progress ring showing time remaining (Green > 48h, Amber 12–48h, Red < 12h).
   - "Fulfill Condition" modal prompting for OREA 124 upload or e-signature dispatch.
4. **Notification Dispatcher:**
   - Send automated reminders at 72h, 24h, and 4h prior to condition expiry.

---

## 5. Verification & QA Test Links

- [ ] `T-008`: Ontario timezone, statutory holiday, and DST condition countdown verification.
