---
title: "TASK-005: External Signed PDF Upload & Reconstruction"
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
  - poc
  - engineering
---

# TASK-005: External Signed PDF Upload & Reconstruction

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-007, TM-017` | QA Case `T-001`

---

## 1. Description & User Story
Allow agents to upload an executed PDF agreement signed outside BROKO, extract key terms, mark verification pending, and gate firm deal transitions on human verification.

---

## 2. Acceptance Criteria
- [ ] Implement core service logic matching specifications.
- [ ] Enforce statutory guardrails and validation rules.
- [ ] Emit audit events for state changes.
- [ ] Connect with dependent workflow states.

---

## 3. Verification Evidence
- [ ] Unit & integration tests written and passing.
- [ ] Manual end-to-end check in Buyer Freehold scenario.
