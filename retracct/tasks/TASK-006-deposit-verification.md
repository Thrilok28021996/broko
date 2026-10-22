---
title: "TASK-006: Deposit Receipt Evidence & Due Date Tracking"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-017]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-011, T-013]]"
tags:
  - task
  - deposit
  - compliance
  - trust-status
  - poc
---

# TASK-006: Deposit Receipt Evidence & Due Date Tracking

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-017` | Decisions `Q20` | QA Cases `T-011`, `T-013` | Ambiguities [[retracct/03-compliance/ambiguities-and-resolutions|A-03]]

---

## 1. Description & User Story

As an agent and brokerage compliance reviewer, I want to track buyer deposit due dates, record deposit receipt events, verify holding party obligations, and attach documentary proof (wire transfer confirmation, certified cheque image, or bank receipt) without running a full general trust ledger, ensuring adherence to Ontario residential transaction norms and TRESA trust accounting notification rules.

---

## 2. Technical Data Contract & Schema

```typescript
export interface DepositTrackingPayload {
  transactionId: string;
  depositRecordId: string;
  depositType: "INITIAL_DEPOSIT" | "ADDITIONAL_DEPOSIT";
  amount: number; // e.g. 50,000.00 CAD
  payableTiming: "HEREWITH" | "UPON_ACCEPTANCE" | "AS_OTHERWISE_DESCRIBED";
  dueTimestamp: string; // ISO 8601 (e.g. 24 hours after agreement acceptance)
  holderClassification: "LISTING_BROKERAGE" | "CO_OPERATING_BROKERAGE" | "SELLER_LAWYER" | "BUYER_LAWYER" | "BUILDER";
  holderEntityName: string; // e.g. "Royal LePage Real Estate Services Ltd., Brokerage"
  status: "EXPECTED" | "INSTRUCTIONS_SENT" | "RECEIVED_CONFIRMED" | "OVERDUE" | "DISPUTED";
  receiptEvidence?: {
    evidenceDocumentId: string; // File UUID (e.g. wire confirmation PDF or certified cheque photo)
    paymentMethod: "WIRE_TRANSFER" | "CERTIFIED_CHEQUE" | "BANK_DRAFT" | "DIRECT_DEPOSIT";
    referenceNumber: string;
    receivedDate: string; // YYYY-MM-DD
    uploadedByUserId: string;
    verifiedByCompliance: boolean;
  };
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Due Date Calculation Engine:**
  - If `payableTiming` is `UPON_ACCEPTANCE`: Computes deadline as precisely 24 hours from contract execution in Ontario timezone (`America/Toronto`), accounting for weekend/holiday guidelines where defined by contract clause.
  - If `AS_OTHERWISE_DESCRIBED`: Computes due date from Schedule A clause parameter (e.g., "within 3 business days of acceptance").
- [ ] **Deposit Decoupling from Firm Status (`Q20`, `T-011`):**
  - An offer can be unconditional and legally firm even if deposit evidence is pending verification.
  - The system must **NOT** hard-block the firm deal status solely due to pending deposit receipt, but must maintain an independent, prominent **Deposit Alert Badge**.
- [ ] **Evidence & Proof Attachment:**
  - Enforces upload of documentary proof (bank draft scan, wire receipt, receipt confirmation from listing brokerage) to mark status as `RECEIVED_CONFIRMED`.
  - Captures payment reference number and receiving institution.
- [ ] **Overdue Escalation Workflow:**
  - Automatically triggers alert notification to Buyer Agent and Broker of Record (BOR) if deposit is not confirmed within 2 hours of due timestamp.
- [ ] **Held Elsewhere Handling (`A-03`, `T-013`):**
  - Properly classifies deposits held by third parties (Seller's Lawyer, Listing Brokerage, or Builder).
  - Adjusts FINTRAC and trust receipt obligations to reflect that our brokerage is not the depository holder.
- [ ] **Audit Trail Events:**
  - `DEPOSIT_EXPECTATION_SCHEDULED`
  - `DEPOSIT_RECEIPT_SUBMITTED`
  - `DEPOSIT_VERIFIED_BY_COMPLIANCE`
  - `DEPOSIT_OVERDUE_ALERT_TRIGGERED`

---

## 4. Implementation Steps

1. **Deposit Service Layer:**
   - Build `DepositManagementService` with methods: `scheduleDeposit()`, `recordReceipt()`, `evaluateOverdueStatus()`.
   - Setup scheduled cron job checking for upcoming and overdue deposits.
2. **Deposit Receipt Upload API:**
   - Endpoint: `POST /api/v1/transactions/{id}/deposits/{depositId}/receipt`.
   - Store evidence document and update deposit state machine.
3. **UI Deposit Countdown & Receipt Card:**
   - Display countdown timer on deal dashboard: "Deposit due in 14h 22m".
   - Status badge transitions: Blue (`Expected`) → Yellow (`Due Soon`) → Green (`Received`) → Red (`Overdue`).
4. **Compliance Reporting:**
   - Include deposit receipt confirmation in the TCS export bundle.

---

## 5. Verification & QA Test Links

- [ ] `T-011`: Deposit independent status test — verify deal transitions to firm independently of deposit status while deposit escalation is raised.
- [ ] `T-013`: Deposit held elsewhere test — verify handling of direct builder/listing brokerage trust deposits without false trust-ledger accounting assumptions.
