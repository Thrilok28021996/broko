---
title: "TASK-002: Representation & RECO Guide Delivery Gate"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-008, TM-009, TM-019]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-004, T-016]]"
tags:
  - task
  - compliance
  - representation
  - tresa
  - poc
---

# TASK-002: Representation & RECO Guide Delivery Gate

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-008`, `TM-009`, `TM-019` | Decisions `Q04`, `Q16`, `Q23` | QA Cases `T-004`, `T-016`

---

## 1. Description & User Story

As a brokerage compliance manager and agent, I want the system to enforce delivery of the mandatory **RECO Information Guide** and require execution of the **Buyer Representation Agreement (OREA Form 300)** or **Self-Represented Party Confirmation (OREA Form 320)** before allowing offer submission, ensuring strict compliance with the **Trust in Real Estate Services Act (TRESA)**.

---

## 2. Technical Data Contract & Schema

```typescript
export interface RepresentationGatePayload {
  transactionId: string;
  recoGuideDelivery: {
    deliveredAt: string; // ISO 8601
    deliveryMethod: "EMAIL" | "IN_PERSON" | "PORTAL_ACKNOWLEDGED";
    clientAcknowledgedAt?: string;
    proofDocumentId?: string; // UUID
  };
  representationAgreement: {
    agreementType: "OREA_300_EXCLUSIVE_BUYER" | "OREA_310_NON_EXCLUSIVE" | "OREA_320_SELF_REPRESENTED";
    effectiveDate: string; // YYYY-MM-DD
    expiryDate: string;    // YYYY-MM-DD
    geographicScope: string; // e.g., "City of Toronto and GTA"
    commissionAgreement: {
      type: "PERCENTAGE" | "FLAT_FEE";
      value: number; // e.g., 2.5%
      holdoverPeriodDays: number; // e.g., 60 days
    };
    designatedAgents: string[]; // Agent UUIDs
    documentVersionId: string;
    signingStatus: "DRAFT" | "SENT" | "EXECUTED";
  };
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Mandatory RECO Guide Receipt:**
  - System captures immutable timestamp and delivery evidence of the RECO Information Guide prior to executing Form 300.
  - Generates compliance flag `STATUTORY_RECO_GUIDE_DELIVERED`.
- [ ] **Representation Agreement Generation (Form 300):**
  - Populates buyer legal names, brokerage legal name, designated agent(s), commencement date, expiry date, geographic area, and commission terms.
  - Automatically flags if representation expiry exceeds 6 months without distinct buyer initialing (statutory Ontario requirement).
- [ ] **Dual-Gate Logic (`Q16` — Safe Drafting vs Regulated Action):**
  - **Permit:** Agents can prepare draft offers and clauses while Form 300 is in signing.
  - **Block:** Hard-block final offer presentation and TCS export if representation agreement is unexecuted.
- [ ] **Self-Represented Party (SRP) Guardrail:**
  - If buyer is marked SRP, enforce delivery of OREA Form 320 warning that brokerage owes no fiduciary duties.
- [ ] **Audit Trail Events:**
  - `RECO_GUIDE_DELIVERED`
  - `REPRESENTATION_EXECUTED`
  - `COMPLIANCE_GATE_EVALUATED`

---

## 4. Implementation Steps

1. **Compliance Service Boundary:**
   - Implement `evaluateRepresentationStatus(transactionId: string): ComplianceStatus`.
   - Maintain non-bypassable guard middleware on `POST /api/v1/offers/{id}/present`.
2. **Document Template Mapping:**
   - Map JSON fields to OREA Form 300 PDF fields (Commencement, Expiry, Commission, Holdover).
3. **UI Representation Status Banner:**
   - Display visual status chip in client transaction view:
     - 🟡 `RECO Guide Awaiting Acknowledgment`
     - 🟠 `Form 300 Pending Execution`
     - 🟢 `Representation Active (Expires YYYY-MM-DD)`
4. **Exception Handling:**
   - Flag warning if expiry is left blank or commission terms contain conflicting provisions.

---

## 5. Verification & QA Test Links

- [ ] `T-004`: Representation model conflict check — ensure designated vs brokerage representation triggers correct disclosures.
- [ ] `T-016`: Safe drafting check — confirm drafting is allowed with pending compliance, but submission is blocked.
