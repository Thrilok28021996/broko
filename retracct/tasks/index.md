---
title: "BROKO Task Management & Sprint Board"
version: "3.0"
date: "2026-10-22"
module: "Task Management"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - tasks
  - sprints
  - kanban
  - roadmap
  - retracct
---

# BROKO Task Management & Sprint Board

Welcome to the task management hub for BROKO. This board tracks delivery milestones from functional requirements (`TM-xxx`), discovery meetings, and compliance rules down to actionable engineering tasks and QA verification.

---

## Active Milestone: Sprint 1 — Buyer Freehold POC (`B01`)

> **Objective:** Deliver an end-to-end working engineering POC for the **Buyer Freehold Resale scenario (`B01`)**, supporting both native drafting (with AI clause assistant) and external signed agreement import, condition fulfillment countdowns, and immutable TCS package export.  
> **Target PRD Requirements:** [[retracct/05-matrices/requirements-matrix|TM-001, TM-002, TM-003, TM-004, TM-005, TM-007, TM-008, TM-009, TM-010, TM-012, TM-013, TM-015, TM-017, TM-019, TM-020, TM-022]]  
> **Target Exit Gates:** [[retracct/04-qa-release/poc-release-plan|POC Acceptance Criteria]]

---

## Master Task Board (Sprint 1 POC)

| Task ID | Task Title | Priority | Status | Requirement | QA Case | Traceability / Focus |
| --- | --- | --- | --- | --- | --- | --- |
| `TASK-001` | [[retracct/tasks/TASK-001-buyer-onboarding\|Buyer Onboarding & Party Classification]] | P0 | `todo` | `TM-001, TM-002` | `T-001, T-004` | Client classification, co-buyers, workspace firewall |
| `TASK-002` | [[retracct/tasks/TASK-002-representation-agreement\|Representation & RECO Guide Delivery Gate]] | P0 | `todo` | `TM-008, TM-009, TM-019` | `T-004, T-016` | TRESA compliance, RECO guide delivery, OREA Form 300 |
| `TASK-003` | [[retracct/tasks/TASK-003-form-100-drafting\|Form 100 Offer Drafting & Schedule A Conditions]] | P0 | `todo` | `TM-003, TM-005, TM-010, TM-013` | `T-003, T-007, T-008` | OREA Form 100, Schedule A builder, dates & calculations |
| `TASK-004` | [[retracct/tasks/TASK-004-esign-dispatch-and-webhook\|E-Signature Dispatch & Webhook Verification]] | P0 | `todo` | `TM-015, TM-023` | `T-010, T-020` | E-signing envelopes, idempotent webhooks, audit certificate |
| `TASK-005` | [[retracct/tasks/TASK-005-signed-pdf-import\|External Signed PDF Upload & Reconstruction]] | P0 | `todo` | `TM-004, TM-007` | `T-001` | External deal import, OCR reconstruction, human verification |
| `TASK-006` | [[retracct/tasks/TASK-006-deposit-verification\|Deposit Receipt Evidence & Due Date Tracking]] | P0 | `todo` | `TM-017` | `T-011, T-013` | 24h statutory deadline, wire proof upload, decoupling |
| `TASK-007` | [[retracct/tasks/TASK-007-condition-fulfillment\|Condition Waiver / Fulfillment & Countdown]] | P0 | `todo` | `TM-013` | `T-008` | Real-time countdown (EST), OREA 123/124, firm deal gate |
| `TASK-008` | [[retracct/tasks/TASK-008-tcs-export\|Transaction Record Sheet (TCS) Package Export]] | P0 | `todo` | `TM-022` | `T-019` | Pre-handoff checklist, compiled audit bundle, API handoff |
| `TASK-009` | [[retracct/tasks/TASK-009-ai-clause-assistant\|AI Clause Assistant & Intelligent Parameterization Engine]] | P1 | `todo` | `TM-012, TM-020` | `T-007, T-017` | Standard 10 OREA clauses, bot prompts, personal vs office |

---

## Task Workflow Stages

1. **Backlog:** Requirements identified in PRD, meetings, or RFCs waiting to be prioritized.
2. **To Do (`todo`):** Groomed tasks with defined technical schemas, acceptance criteria, and QA cases ready for execution.
3. **In Progress (`in-progress`):** Implementation currently underway (code, tests, or docs).
4. **Review / Verification (`review`):** Pull request open, unit/integration tests running, or QA verification pending.
5. **Done (`done`):** Acceptance criteria satisfied and linked QA tests passing.

---

## How to Work on Tasks in OpenKnowledge

### 1. Work on a Task with Antigravity
You can instruct Antigravity directly in chat:
* *"Pick up `TASK-001` and implement the Buyer Onboarding entity schema."*
* *"Update the status of `TASK-003` to `in-progress`."*
* *"Implement the condition countdown service for `TASK-007`."*

### 2. Create a New Task
* **In Desktop App:** Go to `retracct/tasks/`, click **New Document from Template**, and choose **`BROKO Task Card`**.
* **Via Agent:** Instruct the agent to create a new task linked to any requirement in [[retracct/05-matrices/requirements-matrix]].

### 3. Route Tasks from Meetings
When reviewing meeting transcripts in [[retracct/meetings/index|Meetings Hub]] (e.g. [[retracct/meetings/2026-10-15-ai-bots-module|2026-10-15 AI Bots Module]]), promote any recorded action item into a task here.
