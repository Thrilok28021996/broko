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
  - poc
---

# TASK-005: External Signed PDF Upload & Reconstruction

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-004`, `TM-007` | Decisions `Q05`, `Q07`, `Q27` | QA Cases `T-001` | Meetings [[retracct/meetings/2026-10-12-counter-offers-email-ingestion|2026-10-12 Counter-Offers Discovery]]

---

## 1. Description & User Story

As an agent who received an executed agreement or counteroffer signed outside the platform (e.g. from the listing agent via external email or paper scan), I want to upload the signed PDF or forward it via email to a dedicated incoming address (`offers@broko.ai`), so that BROKO automatically parses and extracts key terms (price, parties, conditions, closing date) into an editable verification screen without forcing manual recreation.

---

## 2. Technical Data Contract & Schema

```typescript
export interface DocumentImportPayload {
  transactionId: string;
  sourceType: "EXTERNAL_EXECUTED_PDF" | "MANUAL_SCAN" | "INBOUND_EMAIL_FORWARD";
  inboundEmailMetadata?: {
    forwardedByAgentEmail: string;
    originalSenderEmail: string;
    subjectLine: string;
    receivedAt: string; // ISO 8601
  };
  fileMetadata: {
    fileName: string;
    fileSizeBytes: number;
    mimeType: "application/pdf";
    sha256Checksum: string;
  };
  extractionConfidenceScore: number; // 0.0 to 1.0
  reconstructedTerms: {
    formType: "OREA_FORM_100_PURCHASE_SALE";
    purchasePrice: number;
    depositAmount: number;
    completionDate: string; // YYYY-MM-DD
    requisitionDate: string; // YYYY-MM-DD
    buyersIdentified: string[];
    sellersIdentified: string[];
    detectedConditions: Array<{
      type: "MORTGAGE" | "INSPECTION" | "STATUS_CERTIFICATE" | "OTHER";
      rawText: string;
      dueDate?: string;
      confidence: number;
    }>;
  };
  verificationStatus: "PENDING_AGENT_VERIFICATION" | "VERIFIED_CONFIRMED" | "REJECTED";
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Upload & Checksum Ingestion:**
  - Accepts PDF uploads up to 35MB.
  - Generates immutable SHA-256 fingerprint for document integrity.
- [ ] **Data Reconstruction & OCR Extraction (`Q07`):**
  - Extracts key contract variables (parties, purchase price, deposit schedule, completion date, and Schedule A conditions).
  - Flags each extracted field with an extraction confidence score.
- [ ] **Mandatory Human Verification Gate (`TM-004`, `T-001`):**
  - Uploaded documents are initially marked `PENDING_VERIFICATION`.
  - The system **NEVER** automatically transitions a deal to `FIRM` or `ACCEPTED` solely based on raw upload.
  - Requires the agent or compliance officer to review the side-by-side comparison screen (PDF viewer on left, parsed fields on right) and click "Confirm Reconstructed Terms".
- [ ] **Document Provenance Tracking:**
  - Preserves both the original uploaded binary and the structured metadata in the transaction audit log.
- [ ] **Audit Trail Events:**
  - `EXTERNAL_DOCUMENT_UPLOADED`
  - `DOCUMENT_TERMS_EXTRACTED`
  - `RECONSTRUCTED_TERMS_CONFIRMED`

---

## 4. Implementation Steps

1. **Document Storage & Ingestion Service:**
   - Implement `POST /api/v1/documents/import` handling multipart form uploads.
   - Run virus scan and compute SHA-256 hash.
2. **Parsing Pipeline:**
   - Use PDF extraction service (e.g. PDF parser / OCR) to identify OREA standard text anchors.
   - Normalize monetary amounts, dates, and names.
3. **Side-by-Side Review UI:**
   - Implement split-pane viewer in OpenKnowledge UI:
     - Left pane: Interactive PDF canvas with bounding boxes.
     - Right pane: Form fields pre-filled with extracted data and confidence badges (Green > 90%, Yellow 50–90%, Red < 50%).
4. **Verification Action:**
   - Store verified terms in transaction state and advance transaction to `ACCEPTED_CONDITIONAL` or `ACCEPTED_FIRM` as attested.

---

## 5. Verification & QA Test Links

- [ ] `T-001`: Deal import test — upload executed agreement, verify extraction, confirm human approval gate prevents premature firm deal status.
