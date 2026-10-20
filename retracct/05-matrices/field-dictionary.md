---
title: "Field Dictionary Starter"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - matrix
  - data-dictionary
---

# Field Dictionary Starter

| Entity | Field | Type | Requirement | Validation | Source of truth | Status |
| --- | --- | --- | --- | --- | --- | --- |
| Transaction | transaction_id | UUID | Always | Unique immutable | BROKO | SPECIFIED |
| Transaction | role | Enum | Always | Buyer/Seller/Landlord/Tenant | BROKO | SPECIFIED |
| Transaction | family | Enum | Always | Controlled scenario taxonomy | BROKO | SPECIFIED |
| Party | party_id | UUID | Always | Link to role/transaction | BROKO | SPECIFIED |
| Property | address | Structured address | As applicable | Province, locality, street | Agent/MLS where licensed | FIELD MAPPING NEEDED |
| Offer | offer_id | UUID | Always for offer | Separate per competing offer | BROKO | SPECIFIED |
| Offer | irrevocability_at | Timestamp TZ | Where offer specifies | Store offset, ambiguous rules reviewed | Signed offer | FIELD MAPPING NEEDED |
| Condition | deadline_at | Timestamp TZ | If applicable | Clause-aware calendar and source text | Signed agreement | SPECIFIED |
| Document | form_version_id | Foreign key | Where licensed form | Version warning independent of compliance | Form registry | SPECIFIED |
| Document | source_sha256 | Text | On file storage | Integrity check | Document store | SPECIFIED |
| Deposit | holder_party_id | Reference | When funds expected | Can be external holder | Agreement/evidence | FIELD MAPPING NEEDED |
| TCS | submission_status | Enum | On handoff | No final send with mandatory gaps | Agent validated packet | SPECIFIED |
