---
title: "Open Controlled Ambiguities & Resolution Protocol"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/03-compliance/engineering-compliance|Engineering & Compliance]]"
tags:
  - ambiguities
  - open-decisions
  - risk-management
---

# Open Controlled Ambiguities & Resolution Protocol

Where industry practice, legal interpretations, or brokerage requirements present conflicting directives, BROKO maintains an explicit **Controlled Ambiguity** register.

---

## Ambiguities Register (A-01 to A-12)

| ID | Conflict/uncertainty | Issue | Design resolution | Decision status | Risk |
| --- | --- | --- | --- | --- | --- |
| A-01 | Q12 versus Q16 | Warn-only current-form-version status while preserving legally significant gates | Never stop navigation because latest-version check is pending, but never mislabel form as current; disclose risk, BOR notification and independently enforce actual required signing/compliance rules. | Decision chosen; legal validity of particular form needs advice | High |
| A-02 | GUIDED/FREEFLOW | Guided for common paths; Freeflow for exceptions, but earlier BOR called for both broadly | Provide guided templates to core common scenarios and freeflow selection for all permitted form/task operations, sharing controls; specialty paths may start Freeflow. | Resolved design interpretation | Medium |
| A-03 | FINTRAC receipt of funds | BOR transcript has conflicting general statements regarding holder and buyer vs seller | Base on transaction facts and current FINTRAC reporting/recordkeeping guidance, not a blanket brokerage-side rule. | Compliance mapping remains verification gate | High |
| A-04 | Form 121 vs 124 | Historical project indexes conflict about fulfillment titles | Never generate by number alone; authoritative library title/version approval needed. | Form registry task | High |
| A-05 | Lease agreement | OREA 400 offer-to-lease does not replace Ontario standard lease where required | Include standard residential lease plus exception classification and applicable disclosure/privacy controls. | Verified general source | High |
| A-06 | Residential vs commercial | User explicitly excludes commercial | Exclude commercial trade/lease workflows and commercial-specific documents from releases. | Resolved scope | Low |
| A-07 | Deposit/trust ownership | Full trust accounting excluded from current module | Track statuses/evidence/instructions, send structured events to back office; do not implement bank/trust ledger reconciliation here. | Resolved scope | Medium |
| A-08 | Specialty transactions | All designed, launch scope later | Design rule extension points and scenario briefs; do not represent as full production-validated forms at launch. | Release planning | Medium |
| A-09 | Agent signing vs legal state | Unconditional/conditional statuses are sometimes conflated with deposit and marketing sold | Track evidence-based contract state separate from listing marketing labels and funds state. | BOR transcript + legal review | High |
| A-10 | Identity document collection | BOR unsure about storage of actual government ID scans | Minimize sensitive collection; decide on lawful necessity, access, retention and method-specific evidence before production. | Privacy decision needed | High |
| A-11 | Seller/landlord listing inputs | Actual listing-side form field inventory not supplied | Final field maps depend on licensed sample files, otherwise field schema is a template pending mapping. | Licensed form library needed | High |
| A-12 | Full marketable vs first POC | Master covers all, POC builds only Buyer Freehold | Acceptance and release gate separate for proof of concept and marketed product; no blanket market-ready claim. | Resolved | Medium |

---

## Resolution Protocol
1. **Explicit Tagging:** Every ambiguous behavior is tagged with an ID (`A-01` to `A-12`).
2. **Conservative Default:** In all disputes between convenience and statutory compliance, the platform gates legal status transitions on verified human documentation.
3. **BOR Override:** The Broker of Record retains exclusive authority to resolve compliance ambiguities on a per-brokerage basis.

---
*See [[retracct/05-matrices/open-decisions|Open Decisions Matrix]] for active risk scoring.*
