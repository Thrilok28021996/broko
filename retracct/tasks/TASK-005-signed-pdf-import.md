---
title: "TASK-005: External Signed PDF Upload & Reconstruction"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-004, TM-007]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-001]]"
tags:
  - task
  - import
  - pdf-reconstruction
  - ocr
  - email-ingestion
  - poc
---

# TASK-005: External Signed PDF Upload & Reconstruction

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-004`, `TM-007` | Decisions `Q05`, `Q07`, `Q27` | QA Cases `T-001` | Meetings [[retracct/meetings/2026-10-12-counter-offers-email-ingestion|2026-10-12 Counter-Offers Discovery]]

---

## 1. What This Feature Does (In Plain English)

In real estate, over half of all accepted offers or counteroffers arrive from other brokerages via email (e.g. the listing agent created the contract on their own computer and emailed a signed PDF).

Rather than forcing our agent to retype everything into the system from scratch:
1. **Easy Ingestion:** The agent can simply upload the signed PDF, or **forward the email** directly to a dedicated BROKO address (e.g. `offers@broko.ai` — the popular SkySlope workflow).
2. **Smart Reading:** The system automatically reads the document, extracting the purchase price, deposit amount, closing date, buyer/seller names, and financing/inspection conditions.
3. **Split-Screen Verification:** The system presents a side-by-side screen: the uploaded PDF on the left, and the extracted fields on the right.
4. **Human Confirmation Gate:** The deal is **never** automatically marked as accepted or firm just because a PDF was uploaded. The agent or compliance manager must review the extracted numbers and click **"Confirm Terms"**.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Option A — Upload via Web:** The agent clicks **"Import Signed Agreement"** and drags the PDF into the browser.
2. **Option B — Email Forwarding (SkySlope Style):** The agent receives an email from the listing agent with the signed counteroffer attached. The agent forwards that email to `offers@broko.ai`. The system recognizes the agent's email address and automatically places the contract in their dashboard.
3. **Split-Screen Review Screen:**
   * **Left Side:** Displays the uploaded contract PDF.
   * **Right Side:** Displays the pre-filled fields:
     * Purchase Price: `$1,150,000` (Confidence: 98% 🟢)
     * Deposit: `$50,000` (Confidence: 95% 🟢)
     * Closing Date: `December 15, 2026` (Confidence: 99% 🟢)
     * Schedule A: Detected *Mortgage Condition (5 business days)*.
4. **Edit & Confirm:** The agent reviews the fields against the document, corrects any typos if needed, and clicks **"Confirm Reconstructed Deal"**.
5. **Deal Activated:** The transaction is now active and ready for condition tracking and deposit verification.

---

## 3. Real Estate & Legal Rules (Plain English)

* **Mandatory Human Verification (`Q07`):** The software must never assume OCR or AI is 100% legally correct. A human agent or compliance manager must confirm the extracted terms before the contract is officially recorded.
* **Document Authenticity:** The original uploaded PDF file must be permanently stored in its exact binary form (with a cryptographic fingerprint) so nobody can tamper with it later.
* **Support Inbound Email Routing:** Forwarded emails must match the agent's registered brokerage email so files don't end up in someone else's account.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] Take a sample signed OREA Form 100 PDF and drag it into the "Import Signed Agreement" box.
- [ ] Verify the split-screen viewer opens showing the PDF on the left and form fields on the right.
- [ ] Confirm the price, deposit, and closing date are correctly filled in from the PDF.
- [ ] Verify the deal status stays as "Pending Agent Verification" until you click the confirm button.
- [ ] Click "Confirm Terms" and verify the transaction dashboard opens with the verified terms.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface DocumentImportPayload {
  transactionId: string;
  sourceType: "EXTERNAL_EXECUTED_PDF" | "INBOUND_EMAIL_FORWARD";
  inboundEmailMetadata?: {
    forwardedByEmail: string;
    receivedAt: string;
  };
  fileMetadata: {
    fileName: string;
    fileSizeBytes: number;
    mimeType: "application/pdf";
    sha256Checksum: string;
  };
  reconstructedTerms: {
    purchasePrice: number;
    depositAmount: number;
    completionDate: string;
    requisitionDate: string;
    buyersIdentified: string[];
    sellersIdentified: string[];
  };
  verificationStatus: "PENDING_AGENT_VERIFICATION" | "VERIFIED_CONFIRMED";
}
```

### Endpoints to Implement
* `POST /api/v1/documents/import` — Multipart PDF upload handler.
* `POST /api/v1/inbound-email/gateway` — Inbound webhook for forwarded emails (`offers@broko.ai`).
* `POST /api/v1/documents/{id}/verify` — Human confirmation endpoint advancing status to verified.

### Developer Checklist
- [ ] Compute SHA-256 hash immediately on upload to guarantee document tamper evidence.
- [ ] Build split-pane UI with PDF viewer on left and reactive form inputs on right.
- [ ] Never auto-advance transaction state on upload alone (satisfies QA test `T-001`).
- [ ] Match inbound forwarded email sender against active agent accounts.

</details>
