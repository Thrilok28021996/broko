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

## 1. What This Feature Does (In Plain English)

Once a real estate purchase becomes a **Firm Deal**, the agent must package up the entire file and hand it over to the brokerage conveyancing department so the lawyers can handle closing day and fund transfers.

In Ontario, this package is called the **Transaction Record Sheet (TCS / Trade Record Sheet)**:
1. **Pre-Flight Checklist:** Before exporting, the system runs an automatic check to ensure nothing is missing (e.g. confirms all conditions are fulfilled, deposit receipt is attached, and the buyer's lawyer contact is listed).
2. **One-Click Assembly:** With one click, the system gathers all signed documents and merges them into one neat, chronologically ordered PDF bundle:
   * **Page 1:** The Official Trade Summary (parties, sale price, commission split, closing date, key lawyers).
   * **Page 2+:** The executed RECO Guide acknowledgment, signed Form 300, signed Form 100 with Schedule A, condition fulfillments (Form 124), deposit receipt proof, and FINTRAC ID record.
3. **Tamper-Evident Security:** Generates a permanent digital fingerprint (checksum) guaranteeing that no pages were altered or removed after handoff.
4. **Handoff to Conveyancing:** Sends the clean package directly into the conveyancer's review queue.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Click Submit Deal to Conveyancing:** On a firm transaction, the agent clicks **"Generate TCS Conveyancing Package"**.
2. **Pre-Flight Checklist Appears:**
   * ✅ Deal Status is FIRM
   * ✅ Legal Buyer Names Verified
   * ✅ Deposit Receipt Attached ($50,000 confirmed)
   * ⚠️ Buyer Real Estate Lawyer Contact missing.
3. **Quick Fix:** The agent types in the buyer's lawyer name and law firm (*"Smith & Associates Law, Toronto"*). The warning turns to a green checkmark ✅.
4. **Preview Trade Record Sheet:** The agent previews the generated 1-page Trade Summary sheet.
5. **Download & Submit:** The agent clicks **"Submit to Conveyancing & Download Bundle"**. A clean, merged PDF package downloads instantly to their computer, and the deal moves to **Pending Closing**.

---

## 3. Real Estate & Legal Rules (Plain English)

* **Mandatory Field Gate (`T-019`):** A TCS package must never be exported with missing statutory information (e.g. missing buyer legal name, missing closing date, or unverified deposit). The system hard-blocks incomplete exports until corrected.
* **Chronological Order:** Documents must be compiled in exact chronological sequence (representation $\rightarrow$ offer $\rightarrow$ counteroffers $\rightarrow$ condition fulfillment $\rightarrow$ deposit proof).
* **Focused Boundary (`Q22`):** The TCS module compiles and exports the verified transaction bundle; it does **not** calculate agent commission splits or bank disbursements (those are handled by separate brokerage accounting software).

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] On a conditional deal, try clicking "Generate TCS" and verify the system blocks you (deal must be FIRM first).
- [ ] On a firm deal, leave the buyer lawyer field blank and click "Generate TCS". Verify the pre-flight checklist highlights the missing lawyer in yellow.
- [ ] Fill in the lawyer's name and click "Download Bundle".
- [ ] Open the downloaded PDF: verify Page 1 is the Trade Record Sheet summary, followed by the signed offer, condition fulfillments, and deposit receipt in order.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface TCSPackageExport {
  tcsExportId: string;
  transactionId: string;
  summary: {
    propertyAddress: string;
    salePrice: number;
    closingDate: string;
  };
  parties: {
    buyers: Array<{ legalName: string; phone: string; email: string }>;
    buyerLawyer?: { name: string; firm: string; email: string };
  };
  documentManifest: Array<{
    documentType: string;
    sha256Checksum: string;
  }>;
  packageChecksumSha256: string;
}
```

### Endpoints to Implement
* `POST /api/v1/transactions/{id}/tcs/export` — Validates pre-flight rules, merges documents via `pdf-lib`, and returns signed bundle download URL.

### Developer Checklist
- [ ] Hard-block export with HTTP 400 if status is not `FIRM` or mandatory fields are absent (satisfies QA test `T-019`).
- [ ] Generate single merged PDF bundle with Trade Record Sheet as cover page.
- [ ] Stamp package-level SHA-256 checksum to guarantee audit immutability.

</details>
