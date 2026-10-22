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

## 1. What This Feature Does (In Plain English)

In Ontario real estate, once an offer is accepted, the buyer usually has **24 hours** to deliver the initial deposit (e.g. `$50,000`) into the listing brokerage's real estate trust account.

This task creates the **Deposit Tracker**:
1. **24-Hour Countdown Clock:** The moment an offer is accepted, the system starts a live countdown: *"Deposit due in 23h 45m"*.
2. **Attach Proof of Payment:** When the buyer pays via certified cheque, bank draft, or wire transfer, the agent simply snaps a photo or uploads the bank confirmation receipt.
3. **Overdue Alerts:** If the 24 hours passes and no proof has been uploaded, the system immediately flags the deal as **Overdue** and alerts both the agent and the Broker of Record (brokerage owner).
4. **Third-Party Holders:** If the deposit is being held elsewhere (for example, by the seller's lawyer or a builder instead of our brokerage), the system correctly notes that our brokerage is not holding the money.
5. **Focused Scope:** This feature tracks proof of deposit and deadlines; it does **not** try to be a complicated bank accounting ledger.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Offer Accepted:** As soon as the offer is accepted, a prominent banner appears on the deal dashboard:
   > ⏳ **Deposit Due:** $50,000.00 — Due by tomorrow at 6:00 PM (23h 52m remaining).
2. **Upload Proof of Payment:**
   * The agent clicks **"Record Deposit Receipt"**.
   * Selects payment method: "Bank Draft / Certified Cheque" or "Wire Transfer".
   * Enters the bank reference number.
   * Uploads the photo or PDF receipt from the bank.
3. **Status Turns Green:** The status badge changes to 🟢 **Deposit Received & Verified**.
4. **If Overdue:** If 24 hours expire without proof, the banner turns red: 🔴 **Deposit Overdue (Alert sent to Broker of Record)**.

---

## 3. Real Estate & Legal Rules (Plain English)

* **Statutory Timeline (TRESA s.27):** Under Ontario law, brokerages holding deposits must deposit funds into their statutory trust account within 5 business days of receipt, but purchase contracts typically require payment within 24 hours of acceptance.
* **Firm Deal Decoupling (`Q20`):** In real estate law, a deal can be legally firm even if the deposit proof has not arrived yet. The software must **never** cancel or block a firm deal solely because the deposit is pending; it simply tracks the overdue status and notifies the broker.
* **Held Elsewhere (`A-03`):** If the listing brokerage or lawyer is holding the funds, our brokerage is not liable as the depository holder.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] On an accepted deal, verify the 24-hour countdown timer is ticking down in real time.
- [ ] Click "Record Deposit Receipt", select "Wire Transfer", type a reference number, and upload a receipt image.
- [ ] Confirm the dashboard turns green (*Deposit Received & Verified*).
- [ ] In a test deal where deadline has passed, verify the red *Overdue* badge appears and an alert is visible in the Broker of Record queue.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface DepositTrackingPayload {
  transactionId: string;
  depositRecordId: string;
  amount: number;
  dueTimestamp: string; // ISO 8601 America/Toronto
  holderClassification: "LISTING_BROKERAGE" | "CO_OPERATING_BROKERAGE" | "LAWYER" | "BUILDER";
  status: "EXPECTED" | "RECEIVED_CONFIRMED" | "OVERDUE";
  receiptEvidence?: {
    documentId: string;
    paymentMethod: "WIRE_TRANSFER" | "BANK_DRAFT";
    referenceNumber: string;
    receivedDate: string;
  };
}
```

### Endpoints to Implement
* `POST /api/v1/transactions/{id}/deposits/{depositId}/receipt` — Attaches proof document and updates state.
* Scheduled cron job checking for upcoming (2h warning) and overdue deposits.

### Developer Checklist
- [ ] Compute 24-hour due date from agreement acceptance timestamp.
- [ ] Decouple deposit state from transaction firm state (satisfies QA test `T-011`).
- [ ] Handle deposits held by third parties (satisfies QA test `T-013`).

</details>
