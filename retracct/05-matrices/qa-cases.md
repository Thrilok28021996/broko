---
title: "QA Test Cases Matrix (T-001–T-026)"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - matrix
  - qa-cases
---

# QA Test Cases Matrix (T-001–T-026)

| Test ID | Requirement ID | Scenario / steps | Expected result |
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
