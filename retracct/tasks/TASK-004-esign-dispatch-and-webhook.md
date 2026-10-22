---
title: "TASK-004: E-Signature Dispatch & Webhook Verification"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-015, TM-023]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-010, T-020]]"
tags:
  - task
  - esign
  - webhooks
  - integrations
  - poc
---

# TASK-004: E-Signature Dispatch & Webhook Verification

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-015`, `TM-023` | Decisions `Q17`, `Q27` | QA Cases `T-010`, `T-020`

---

## 1. What This Feature Does (In Plain English)

Once an offer or representation contract is drafted, it needs to be signed by the buyers.

This task sets up the **Electronic Signature** system:
1. **Send for Signature:** With one click, the agent sends the contract to the buyer's email so they can review and sign it on their phone, tablet, or laptop.
2. **Signature Placement:** The system automatically places the signature lines, initials, and date tags in the exact right spots on the OREA forms so the buyer doesn't miss any initials.
3. **Real-Time Tracking:** The agent can see in real-time when the buyer has opened the document and signed it.
4. **Legal Certificate:** When everyone finishes signing, the system automatically pulls back the completed PDF along with a tamper-evident audit certificate (recording IP address, timestamp, and identity proof) for Ontario legal compliance.
5. **Fallback:** If an e-sign provider has a technical glitch, the agent can easily switch to manual signing without losing any of the typed terms.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Click Send for Signing:** On the completed Form 100 offer, the agent clicks **"Send for E-Signature"**.
2. **Review Signers:** The screen lists the buyers (e.g. Buyer 1 and Buyer 2) with their email addresses. The agent confirms the signing order.
3. **Buyer Experience:**
   * The buyer receives an email: *"Your agent has prepared an offer for 123 Maple Street. Click here to review and sign."*
   * The buyer clicks the link, initials each page of Form 100 and Schedule A, signs the signature block, and clicks **Finish**.
4. **Agent Dashboard Live Updates:**
   * Dashboard shows progress: ✉️ *Sent* $\rightarrow$ 👁️ *Viewed by Buyer 1* $\rightarrow$ ✍️ *Signed by Buyer 1 (1/2)* $\rightarrow$ 🟢 *Fully Executed*.
5. **Completed Documents Stored:** Once complete, the executed PDF and the audit trail certificate are safely saved in the transaction files.

---

## 3. Real Estate & Legal Rules (Plain English)

* **Ontario Electronic Commerce Act Compliance:** Digital signatures are 100% legally binding in Ontario real estate, provided the platform retains a verified audit log showing who signed, when, and from what IP address.
* **All Initials Required:** Every page of OREA Form 100 has a mandatory initial box at the bottom. The system must not allow the buyer to submit until every single page is initialed.
* **Network Glitch Safety:** If the e-signature service sends duplicate completion messages over the internet, the system must process it cleanly without duplicating the document in the file.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] In a draft offer, click "Send for E-Signature" to send to your own test email.
- [ ] Open the email on your phone, click the link, and verify initial and signature tabs are highlighted in yellow on Form 100.
- [ ] Sign the document and click finish.
- [ ] Return to the agent screen and confirm the status has automatically turned to "Fully Signed & Verified".
- [ ] Open the completed document from the files tab and verify the audit certificate is attached to the back.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface ESignEnvelopeDispatchPayload {
  transactionId: string;
  documentId: string;
  envelopeTitle: string;
  signers: Array<{
    partyId: string;
    role: "BUYER_1" | "BUYER_2";
    name: string;
    email: string;
    signingOrder: number;
  }>;
}

export interface ESignWebhookEventPayload {
  eventId: string;
  provider: "DOCUSIGN" | "SIGNNOW" | "NATIVE";
  envelopeId: string;
  eventType: "envelope.completed";
  signedPdfDownloadUrl: string;
  certificateHashSha256: string;
}
```

### Endpoints to Implement
* `POST /api/v1/integrations/esign/dispatch` — Prepares tabs and creates provider envelope.
* `POST /api/v1/integrations/esign/webhook` — HMAC validated webhook handler with Redis idempotency lock.

### Developer Checklist
- [ ] Pre-map OREA Form 100 initial coordinates (pages 1–6) and signature block (page 5).
- [ ] Store provider certificate hash and downloaded PDF in encrypted storage.
- [ ] Deduplicate webhooks using Redis `SETNX esign_event:{eventId}` lock.
- [ ] Automated tests passing for QA cases `T-010` and `T-020`.

</details>
