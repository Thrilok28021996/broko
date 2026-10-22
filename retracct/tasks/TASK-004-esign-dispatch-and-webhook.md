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

## 1. Description & User Story

As an agent or client signer, I want to dispatch drafted offer packages (Form 100 + Schedule A) for electronic signatures and automatically receive tamper-evident completion certificates and signed PDF documents via idempotent webhooks, ensuring strict auditability under Ontario's Electronic Commerce Act.

---

## 2. Technical Data Contract & Schema

```typescript
export interface ESignEnvelopeDispatchPayload {
  transactionId: string;
  documentId: string;
  envelopeTitle: string; // e.g. "Form 100 - 123 Maple Street - Buyer Offer"
  signers: Array<{
    partyId: string;
    role: "BUYER_1" | "BUYER_2" | "BUYER_AGENT";
    legalName: string;
    email: string;
    phoneForSMSAuth?: string;
    signingOrder: number;
    signaturePlacement: Array<{
      pageNumber: number;
      xCoord: number;
      yCoord: number;
      type: "SIGNATURE" | "INITIAL" | "DATE";
    }>;
  }>;
  webhookCallbackUrl: string;
  expiresInDays: number; // e.g. 3 days or matching irrevocable time
}

export interface ESignWebhookEventPayload {
  eventId: string;
  provider: "DOCUSIGN" | "SIGNNOW" | "NATIVE_ESIGN";
  envelopeId: string;
  eventType: "envelope.sent" | "envelope.delivered" | "recipient.completed" | "envelope.completed" | "envelope.declined";
  timestamp: string; // ISO 8601
  certificateHashSha256: string;
  downloadSignedPdfUrl: string;
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Envelope Generation & Placement:**
  - Generates signing envelope with pre-mapped signature tabs on Form 100 (Page 1 initials, confirmation initials, and signature block Page 5/6).
  - Enforces sequential or parallel signing order for multi-party buyers.
- [ ] **Idempotent Webhook Processing (`TM-023`, `T-020`):**
  - Webhook endpoint (`POST /api/v1/integrations/esign/webhook`) validates provider HMAC signature before processing.
  - Dedupes duplicate delivery events using `eventId` cache.
  - Safely transitions document state from `SENT` → `PARTIALLY_SIGNED` → `VERIFIED_EXECUTED`.
- [ ] **Certificate & Proof Retention:**
  - Automatically downloads and attaches the independent signing certificate (audit trail showing IP address, email, timestamp, and cryptographically verified hash).
  - Stores signed PDF in encrypted document vault with SHA-256 fingerprinting.
- [ ] **Fallback / Failure Handling (`T-010`):**
  - If e-signing provider experiences an outage or email bounce, allows agent to cancel envelope and pivot to external manual signing without losing drafted form data.
- [ ] **Audit Trail Events:**
  - `ESIGN_ENVELOPE_DISPATCHED`
  - `RECIPIENT_SIGNED`
  - `DOCUMENT_EXECUTION_COMPLETED`
  - `SIGNATURE_CERTIFICATE_VERIFIED`

---

## 4. Implementation Steps

1. **Integration Adapter Layer:**
   - Define `ESignatureProvider` interface (`createEnvelope`, `getEnvelopeStatus`, `downloadSignedDocument`).
   - Implement provider adapter with mock mode for unit tests and local development.
2. **Webhook Controller:**
   - Construct `/api/v1/integrations/esign/webhook` with replay-attack protection and HMAC validation.
   - Use redis-backed deduplication lock.
3. **Storage Pipeline:**
   - Stream downloaded signed PDF directly to private S3/GCS bucket; compute SHA-256 checksum.
4. **Front-End Status Widget:**
   - Display real-time signing progression:
     - ✉️ Envelope Sent
     - 👁️ Opened by Buyer 1
     - ✍️ Signed by Buyer 1 (1/2)
     - ✅ Fully Executed & Verified

---

## 5. Verification & QA Test Links

- [ ] `T-010`: Provider failure fallback test — verify system safely handles provider interruptions and permits fallback without corruption.
- [ ] `T-020`: Duplicate callback idempotency test — simulate duplicate webhook delivery and verify only one state transition occurs.
