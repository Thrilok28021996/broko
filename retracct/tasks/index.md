---
title: "BROKO Task Management & Sprint Board"
version: "3.0"
date: "2026-10-20"
module: "Task Management"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - tasks
  - sprints
  - kanban
  - roadmap
---

# BROKO Task Management & Sprint Board

Welcome to the task management hub for BROKO. This board tracks delivery milestones from functional requirements (`TM-xxx`) and discovery meetings down to actionable engineering tasks and QA verification.

---

## Active Milestone: Sprint 1 — Buyer Freehold POC (`B01`)

> **Objective:** Deliver an end-to-end working engineering POC for the Buyer Freehold Resale scenario (`B01`), supporting both native drafting and external signed agreement import.  
> **Target PRD Requirements:** [[retracct/05-matrices/requirements-matrix|TM-001, TM-002, TM-003, TM-005, TM-017, TM-019, TM-020, TM-022]]  
> **Target Exit Gates:** [[retracct/04-qa-release/poc-release-plan|POC Acceptance Criteria]]

---

## Master Task Board

| Task ID | Task Title | Priority | Status | Requirement | QA Case | Assignee |
| --- | --- | --- | --- | --- | --- | --- |
| `TASK-001` | [[retracct/tasks/TASK-001-buyer-onboarding\|Buyer Onboarding & Party Classification]] | P0 | `todo` | `TM-001, TM-002` | `T-001` | Eng Team |
| `TASK-002` | [[retracct/tasks/TASK-002-representation-agreement\|Representation & RECO Guide Delivery Gate]] | P0 | `todo` | `TM-002, TM-023` | `T-002` | Eng Team |
| `TASK-003` | [[retracct/tasks/TASK-003-form-100-drafting\|Form 100 Offer Drafting & Schedule A Conditions]] | P0 | `todo` | `TM-003, TM-019` | `T-003` | Eng Team |
| `TASK-004` | [[retracct/tasks/TASK-004-esign-dispatch-and-webhook\|E-Signature Dispatch & Webhook Verification]] | P0 | `todo` | `TM-017` | `T-006` | Eng Team |
| `TASK-005` | [[retracct/tasks/TASK-005-signed-pdf-import\|External Signed PDF Upload & Reconstruction]] | P0 | `todo` | `TM-007, TM-017` | `T-001` | Eng Team |
| `TASK-006` | [[retracct/tasks/TASK-006-deposit-verification\|Deposit Receipt Evidence & Due Date Tracking]] | P0 | `todo` | `TM-020` | `T-009` | Eng Team |
| `TASK-007` | [[retracct/tasks/TASK-007-condition-fulfillment\|Condition Waiver / Fulfillment & Countdown]] | P0 | `todo` | `TM-019` | `T-008` | Eng Team |
| `TASK-008` | [[retracct/tasks/TASK-008-tcs-export\|Transaction Record Sheet (TCS) Package Export]] | P0 | `todo` | `TM-022` | `T-010` | Eng Team |

---

## Task Workflow Stages

1. **Backlog:** Requirements identified in PRD, meetings, or RFCs waiting to be prioritized.
2. **To Do (`todo`):** Groomed tasks with defined acceptance criteria ready for execution.
3. **In Progress (`in-progress`):** Implementation currently underway (code, tests, or docs).
4. **Review / Verification (`review`):** Pull request open or QA verification pending.
5. **Done (`done`):** Acceptance criteria satisfied and linked QA tests passing.

---

## How to Manage Tasks in OpenKnowledge

### 1. Work on a Task with Antigravity
You can instruct Antigravity directly in chat:
* *"Pick up `TASK-001` and implement the Buyer Onboarding entity schema."*
* *"Update the status of `TASK-003` to `in-progress`."*
* *"Run tests for `TASK-006` and mark it `done`."*

### 2. Create a New Task
* **In Desktop App:** Go to `retracct/tasks/`, click **New Document from Template**, and choose **`BROKO Task Card`**.
* **Via Agent:** Say *"Create a new task for FINTRAC client ID verification linked to TM-023."*

### 3. Route Tasks from Meetings
When reviewing meeting transcripts in [[retracct/meetings/index|Meetings Hub]], promote any recorded action item into a task here.
