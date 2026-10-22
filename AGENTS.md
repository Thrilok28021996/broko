# Agent Guidelines — BROKO Project

Welcome to the **BROKO** repository. This codebase and knowledge base manage transaction and offer workflows for Ontario residential real estate under the **Trust in Real Estate Services Act (TRESA)**.

To ensure deterministic, consistent results across all team members and AI coding assistants (Antigravity, Cursor, Claude Code, GitHub Copilot, Codex, Gemini CLI), you must strictly follow these instructions.

---

## 1. Domain Skill & Source of Truth

* **Canonical Skill:** Read and follow [`.agents/skills/broko-retracct/SKILL.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/.agents/skills/broko-retracct/SKILL.md) for all architectural rules, reconciled decisions (Q01–Q30), and clause parameterization flows.
* **Knowledge Base Root:** [`retracct/index.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/index.md)
* **Authoritative Matrices:**
  * Reconciled Decisions: [`retracct/05-matrices/decision-register.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/05-matrices/decision-register.md)
  * Functional Requirements: [`retracct/05-matrices/requirements-matrix.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/05-matrices/requirements-matrix.md)
  * Open Ambiguities: [`retracct/03-compliance/ambiguities-and-resolutions.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/03-compliance/ambiguities-and-resolutions.md)
  * QA Test Cases: [`retracct/05-matrices/qa-cases.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/05-matrices/qa-cases.md)

---

## 2. Universal Operational Boundaries

1. **Jurisdiction:** Ontario Residential Real Estate only (OREA / RECO TRESA). Commercial real estate is explicitly excluded.
2. **Current Target:** **Sprint 1 — Buyer Freehold Resale POC (`B01`)** (Tasks `TASK-001` through `TASK-009`).
3. **Out of Scope:** Brokerage general CRM, trust ledger accounting / fund disbursements, commission payout calculations.
4. **Source Fidelity:** Never invent legal requirements, statutory timeframes, or OREA form numbers. Unverified forms display a persistent warning banner (`Q12`) without blocking safe drafting (`Q16`).
5. **Human-in-the-Loop:** AI-generated clauses or custom drafts must be confirmed by a human agent before appending to Schedule A (`Q14`). No contract is autonomously forfeited upon a missed deadline (`Q19`).
6. **Privacy:** No client PII (driver's licenses, SIN numbers, banking details) may be sent to unapproved external AI models (`Q26`, `TM-020`).

---

## 3. Sprint 1 Task Registry

All engineering deliverables must be tracked and updated in [`retracct/tasks/`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/tasks/):
* `TASK-001`: Buyer Onboarding & Party Classification
* `TASK-002`: Representation & RECO Guide Delivery Gate
* `TASK-003`: Form 100 Offer Drafting & Schedule A Conditions
* `TASK-004`: E-Signature Dispatch & Webhook Verification
* `TASK-005`: External Signed PDF Upload & Reconstruction
* `TASK-006`: Deposit Receipt Evidence & Due Date Tracking
* `TASK-007`: Condition Waiver / Fulfillment & Countdown
* `TASK-008`: Transaction Record Sheet (TCS) Package Export
* `TASK-009`: AI Clause Assistant & Intelligent Parameterization Engine
