---
title: "TASK-008: Transaction Record Sheet (TCS) Package Export"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-022]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-019]]"
tags:
  - task
  - tcs
  - handoff
  - compliance
  - conveyancing
  - poc
---

# TASK-008: Transaction Record Sheet (TCS) Package Export

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-022` | Decisions `Q22`, `Q24` | QA Cases `T-019`

---

## 1. Technical Objective & User Context

### Technical Objective (What Developers Build)
Build the conveyancing pre-flight checklist validator, chronological PDF document bundle merger (using `pdf-lib`), immutable SHA-256 package snapshot generator, and the structured TCS export endpoint (`POST /tcs/export`) with accounting handoff event dispatching.

### End-User Persona
* **Primary User:** Licensed Real Estate Agent (submitting deal package)
* **Recipient User:** Brokerage Conveyancer / Back-Office Accounting Administrator

### Business Rationale & Domain Context
Once a transaction goes firm, brokerage conveyance staff require a complete, tamper-evident Transaction Record Sheet (Trade Record Sheet) containing all signed contracts, fulfillment notices, deposit proofs, and FINTRAC identification. Under PRD Decision `Q22`, this must be a validated, read-only snapshot ready for handoff without performing commission disbursement calculations (`T-019`).

---

## 2. Technical Data Contract & Schema

```typescript
export interface TCSPackageExport {
  tcsExportId: string; // UUID
  transactionId: string;
  compiledAt: string; // ISO 8601
  compiledByUserId: string;
  transactionStatus: "FIRM" | "CLOSED";
  summary: {
    mlsNumber?: string;
    propertyAddress: string;
    legalDescription: string;
    salePrice: number;
    closingDate: string; // YYYY-MM-DD
    possessionTime: string;
  };
  parties: {
    buyers: Array<{ legalName: string; address: string; phone: string; email: string }>;
    sellers: Array<{ legalName: string; address?: string }>;
    buyerLawyer?: { lawyerName: string; lawFirm: string; email: string; phone: string };
    sellerLawyer?: { lawyerName: string; lawFirm: string; email: string; phone: string };
  };
  brokerageDetails: {
    cooperatingBrokerage: { name: string; designatedAgent: string; commissionShare: string };
    listingBrokerage: { name: string; listingAgent: string; commissionShare: string };
  };
  financialSummary: {
    purchasePrice: number;
    initialDeposit: { amount: number; heldBy: string; receiptVerified: boolean };
    balanceDueOnClosing: number;
  };
  documentManifest: Array<{
    documentType: "RECO_GUIDE_ACK" | "OREA_300" | "OREA_100_APS" | "OREA_124_FULFILLMENT" | "DEPOSIT_RECEIPT" | "FINTRAC_CREA";
    documentTitle: string;
    fileSha256Checksum: string;
    executionTimestamp: string;
  }>;
  packageChecksumSha256: string;
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Strict Pre-Handoff Validation (`TM-022`, `T-019`):**
  - Evaluates mandatory conveyancing prerequisites before enabling export:
    1. Deal status must be `FIRM` (all conditions fulfilled or waived).
    2. Mandatory buyer contact and legal names present.
    3. Mandatory legal property description present.
    4. Deposit receipt evidence verified.
    5. Buyer lawyer contact information recorded (or explicit pending flag).
  - If any mandatory field is missing, hard-blocks export and displays clear field-level remediation errors (`T-019`).
- [ ] **Immutable Read-Only Snapshot:**
  - Freezes package contents into an immutable, tamper-evident manifest with individual and package-level SHA-256 hashes.
- [ ] **Chronological Document Bundle Generation:**
  - Compiles a single merged PDF package including:
    1. Page 1: Brokerage Standard Transaction Record Sheet (Trade Summary).
    2. Executed RECO Guide Acknowledgment.
    3. Executed Form 300 (Buyer Representation).
    4. Executed Form 100 (APS) + Schedule A.
    5. Executed Form 124 Notices of Fulfillment / Form 123 Waivers.
    6. Deposit bank wire receipt / draft evidence.
    7. FINTRAC Individual Identification information record.
- [ ] **Handoff API Endpoint (`POST /api/v1/tcs/packages`):**
  - Dispatches structured JSON payload to back-office accounting / conveyancing systems.
  - Idempotent and safe for retries.
- [ ] **Audit Trail Events:**
  - `TCS_VALIDATION_EVALUATED`
  - `TCS_PACKAGE_GENERATED`
  - `TCS_HANDOFF_DISPATCHED`

---

## 4. Implementation Steps

1. **Validation Engine:**
   - Create `TCSValidationRuleEngine` checking all mandatory Ontario trade record fields.
2. **PDF Packaging Service:**
   - Implement PDF merger using `pdf-lib` to assemble bundle and stamp header page with deal metadata and checksum.
3. **Export Controller & API:**
   - Endpoint `POST /api/v1/transactions/{id}/tcs/export`.
   - Generates downloadable ZIP/PDF and publishes `TCS_PACKAGE_GENERATED`.
4. **UI Conveyancing Handoff Screen:**
   - Step 1: Pre-flight checklist (shows green checkmarks for all prerequisites).
   - Step 2: Preview generated Trade Record Sheet.
   - Step 3: "Submit to Conveyancing & Download Package" button.

---

## 5. Verification & QA Test Links

- [ ] `T-019`: Mandatory field missing test — submit TCS with missing required field (e.g. buyer lawyer or deposit proof) and verify handoff is gated pending correction.
