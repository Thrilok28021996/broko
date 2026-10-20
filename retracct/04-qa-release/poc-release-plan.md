---
title: "POC Scope, Release Plan & Acceptance"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - poc
  - release-plan
  - acceptance
---

# BROKO — POC Scope, Release Plan & Acceptance

> **Buyer Freehold first; residential market module later**  
> Companion documents:
> - [[retracct/04-qa-release/qa-test-plan|QA Test Plan & Verification Strategy]]
> - [[retracct/05-matrices/qa-cases|QA Cases (T-001–T-026)]]

---

**BROKO**

**04 — Release Plan, POC & Acceptance**

Buyer Freehold first; residential market module later

Final requirements baseline for design | 20 October 2026 | Ontario residential only | Not a certification of legal compliance


# 1. Separation of complete design from delivery

Full-module PRD covers all proposed residential roles and specialty scenarios. The first POC implements only Buyer Freehold end-to-end. Market-release dates and whether specialty cases ship initially remain intentionally uncommitted; prioritize by brokerage demand and validated form availability. Commercial is excluded.


| Phase | Deliverable | Exit gate |
| --- | --- | --- |
| P0 Buyer Freehold POC | Create/import buyer, representation, Form 100 offer+Schedule A/B, signing, offer/counteroffer, condition/reminder, amendment, firm/closing status, deposit events, TCS handoff | Positive and negative Buyer Freehold scenarios pass; BOR witnesses an end-to-end demonstration |
| R1 Residential common cases | Buyer/Seller Freehold + Condo resale, Landlord/Tenant standard residential lease, listing-side, multi-offer register, client permissions, privacy, complete clause/field rules | Every core path has tested forms, rights/role gates and operational support |
| R2 Specialty residential | POTL, builder/preconstruction, assignment, power-of-sale, rural/vacant land, investment property, room/student tenancy, renewal variations | Applicable custom/third-party forms licensed and verified; exceptions tested |
| Production hardening | Access, monitoring, recovery, field mapping, audit, retention, licensed integration, mobile responsive | Independent security/privacy QA, performance SLOs, BOR and legal/compliance approval |


# 2. POC scope and explicit exclusions

- POC must demonstrate onboarding → representation → offer package → signed presentation → counter → conditional acceptance → notice/waiver/amendment → firm → deposit tracking → closing/outcome → validated TCS package.
- POC must also import an already-executed agreement and resume from an intermediate step, demonstrating no forced recreation.
- POC does not implement production seller, condo or landlord/tenant workflow; but core engine and domain taxonomy must not prevent them.
- POC excludes commercial, full trust accounting, commission payout engine, CRM, billing and brokerage SaaS administration.

# 3. Test plan


| Test ID | Requirement | Scenario | Pass criterion |
| --- | --- | --- | --- |
| T-001 | TM-001 | Create from accepted deal, upload signed agreement; reconstruct event history | Imported docs pending verification; only verified prerequisites satisfy gates |
| T-002 | TM-003 | Select dependent Form B directly in Freeflow with Form A missing | Offer prerequisite completion or verified upload; no false completion |
| T-003 | TM-006 | Form version unknown; continue drafting and signing | Warnings remain visible; BOR notified; never marked version-verified; statutory checks independent |
| T-004 | TM-008 | Same brokerage represents two parties; conflict triggers | Client workspaces isolated; mandated disclosure and consent flow applied |
| T-005 | TM-010 | Three competing purchase offers arrive; one countered and accepted | Register preserves all versions, presentation dates, outcomes and parties |
| T-006 | TM-011 | Offer expiry passes or acceptance disputed | No automatic legally binding conclusion; review queue and evidence |
| T-007 | TM-012 | AI drafts custom inspection clause | Draft flagged for agent/BOR approval; no silent contractual insertion |
| T-008 | TM-013 | One business-day condition and one calendar-day condition; holiday and DST | Separate accurate schedules with clause-specific rules and Ontario timezone |
| T-009 | TM-014 | Change buyer name after execution | Original immutable; amendment/signatures and dependent data review |
| T-010 | TM-015 | Signing provider fails mid-document; agent uploads signed fallback | Preserve in-flight state, no duplicates, validate external signing evidence |
| T-011 | TM-016 | Deposit absent though offer unconditional and executed | No deposit-only firm gate; deposit escalations tracked independently |
| T-012 | TM-016 | Closing delayed, later fails | Separate delayed/failed closing and authorized termination tasks |
| T-013 | TM-017 | Deposit held elsewhere / direct builder payment | Status and applicable records evaluated based on actual event, not guessed holder |
| T-014 | TM-018 | Tenant applies; landlord declines after human review | Screening data isolated; decision audit recorded; no automated prohibited reasoning |
| T-015 | TM-018 | Ontario covered tenancy uses offer-to-lease then standard lease | Relevant standard lease task generated; exceptions handled |
| T-016 | TM-019 | Compliance record missing but draft requested | Draft allowed where safe; compliance status remains incomplete |
| T-017 | TM-020 | Unapproved model requests client identity PDF | Prevent transfer; produce audit/security event |
| T-018 | TM-021 | Seven-year period altered; legal hold active | Retention configuration changes without deleting held documents |
| T-019 | TM-022 | Agent submits TCS with missing required field | Structured handoff gated pending correction |
| T-020 | TM-023 | Duplicate signing callback / email retry | Idempotent handler; one business transition |
| T-021 | TM-025 | Listing changes price and is later withdrawn | Versioned listing terms and documented withdrawal/termination |
| T-022 | TM-028 | Designated representative views opposite-party restricted documents | Access denied and logged |
| T-023 | TM-027 | Power of sale with bank agreement replacing normal APS | Bank docs and exceptions reviewed; not silently inherited standard pack |
| T-024 | TM-024 | Agent on phone reviews deadline and uploads required evidence | Responsive UI, accessible controls and meaningful task status |
| T-025 | TM-029 | Signed amendment invalidates earlier prerequisite | Dependencies re-evaluated; history preserved |
| T-026 | TM-030 | Deadline reminder delivery fails | Failure visible and safe retry/escalation recorded |


# 4. Market-release gate checklist

- Every marketed transaction class has exact applicable forms and current licensed field mappings, conditional branches and exception tests.
- RECO/FINTRAC/tenancy source-linked rules checked for production effective dates; legal ambiguities resolved by reviewer and recorded.
- Warn-only form-version policy produces visible BOR alerts and explicit unresolved verification state; cannot forge compliance completion.
- Both-sides-of-trade permissions independently penetration-tested and client confidentiality preserved.
- All e-sign/import failure recovery, counteroffers, time-zone/holiday rules, mobile views and TCS integration verified.
- Tenant screening data minimized and human-decision process tested for privacy/fairness compliance.
- Support desk, incident response, backup restore and operational monitoring tested and documented.

# 5. Sources and BOR decision ownership

Q01–Q30 are user-authorized product decisions. The October 19 BOR discussion anchors Guided/Freeflow, form intent metadata, clause approach, firm states, offers, amendments, deposits, TCS and retention. Source-backed external checks are identified as REG-01–REG-06. Unresolved legal facts are controlled production gates, not assumed approvals.
