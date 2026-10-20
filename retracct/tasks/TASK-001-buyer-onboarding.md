---
title: "TASK-001: Buyer Onboarding & Party Classification"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix]]"
qa_case: "[[retracct/05-matrices/qa-cases]]"
tags:
  - task
  - onboarding
  - poc
---

# TASK-001: Buyer Onboarding & Party Classification

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-001`, `TM-002` | QA Case `T-001`

---

## 1. Description & User Story
As an agent, I want to onboard a new residential buyer client and classify their transaction context (role, property type, representation status, and co-buyers), so that the platform configures the proper legal forms and validation rules.

---

## 2. Acceptance Criteria
- [ ] Create transaction with unique UUID and role `Buyer`.
- [ ] Classify property type as `Freehold Resale`.
- [ ] Record buyer identity details (legal name, contact info, representation model).
- [ ] Emit `TRANSACTION_CREATED` and `CLIENT_ASSIGNED` events.
- [ ] Initialize workspace permissions firewalled to the buyer agent and authorized staff.

---

## 3. Implementation Checklist
- [ ] Define Buyer transaction data schema.
- [ ] Implement `POST /transactions` endpoint handler.
- [ ] Add unit tests for invalid roles or duplicate client entries.
- [ ] Verify event persistence in audit log.
