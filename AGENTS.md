# Agent Guidelines — BROKO Platform

Welcome to the **BROKO** repository. BROKO is a modular real estate enterprise platform engineered for the Ontario market under the **Trust in Real Estate Services Act (TRESA)**.

This document is the **root entry point** for all AI coding assistants (Antigravity, Cursor, Claude Code, GitHub Copilot, Codex, Gemini CLI) and human contributors. It establishes platform-level principles, universal guardrails, and the routing mechanism for working across BROKO's modular architecture.

---

## 1. Modular Platform Architecture & Routing Matrix

BROKO is partitioned into autonomous modules. Rather than maintaining a monolithic instruction set, each module owns its dedicated knowledge base, functional requirements, and domain skill.

When working within a specific module directory, **you must activate and follow that module's dedicated skill**:

| Module Directory | Module Name | Current Status | Knowledge Base Root | Dedicated Domain Skill | Scope Summary |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **`retracct/`** | **Offer & Transaction Management** | **Active (Sprint 1 POC)** | [`retracct/index.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/index.md) | [`.agents/skills/broko-retracct/SKILL.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/.agents/skills/broko-retracct/SKILL.md) | Form drafting (100/101), e-signatures, counteroffers, deposit proof, conditions & TCS package export. |
| **`crm/`** | Agent CRM & Lead Gen | *Planned (Post-POC)* | `crm/index.md` *(future)* | `.agents/skills/broko-crm/SKILL.md` *(future)* | Client database, lead routing, touchpoints, marketing automation. |
| **`accounting/`** | Trust Ledger & Commissions | *Planned (Post-POC)* | `accounting/index.md` *(future)* | `.agents/skills/broko-accounting/SKILL.md` *(future)* | Trust general ledger, fund disbursements, commission splits, banking reconciliations. |
| **`portal/`** | Client Collaborative Portal | *Planned (Post-POC)* | `portal/index.md` *(future)* | `.agents/skills/broko-portal/SKILL.md` *(future)* | Client-facing portal for document review, e-signing, and condition milestone viewing. |

> [!TIP]
> **Contextual Dispatch:** When a task involves files under `retracct/`, immediately load and prioritize [`.agents/skills/broko-retracct/SKILL.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/.agents/skills/broko-retracct/SKILL.md). When new modules are added, follow the module extension protocol below.

---

## 2. Universal Platform Boundaries (Inherited by ALL Modules)

Regardless of which module you are modifying, these platform-level guardrails are strictly non-negotiable:

1. **Jurisdiction & Legal Framework:**
   - Ontario Residential Real Estate only (governed by RECO, OREA, and TRESA). Commercial real estate is explicitly out of scope.
2. **Source Fidelity & No Invention Rule:**
   - **Never invent** legal requirements, statutory timeframes, OREA form numbers, or clause texts.
   - Incomplete or unverified forms must display a non-blocking warning banner (`Q12`) without falsely claiming legal certification (`Q16`).
3. **Human-in-the-Loop Safeguards:**
   - AI proposals (clause recommendations, PDF extractions, compliance checks) are always suggestions requiring licensed human licensee confirmation (`Q14`).
   - The platform **never autonomously terminates or forfeits contracts** upon missed deadlines (`Q19`); it transitions conditions to `EXPIRED_UNRESOLVED` and alerts users.
4. **Data Privacy & AI Model Boundaries (`Q26`, `TM-020`):**
   - Client identity documents (driver's licenses, passports, SIN cards) and confidential banking details must **never** be transmitted to external unapproved LLMs or third-party APIs.
5. **Deterministic Timezone & Calendar Computation:**
   - All offer expirations, condition countdowns, and completion dates must be evaluated strictly in the `America/Toronto` timezone, respecting Ontario statutory holidays and land registry office closures (`Q18`).

---

## 3. Active Target: Sprint 1 (`retracct` Module)

The current active engineering milestone across the repository is the **Buyer Freehold Resale POC (`B01`)** inside the `retracct` module.

All active tasks for this milestone are tracked in [`retracct/tasks/`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/tasks/):
* `TASK-001`: Buyer Onboarding & Party Classification
* `TASK-002`: Representation & RECO Guide Delivery Gate
* `TASK-003`: Form 100 Offer Drafting & Schedule A Conditions
* `TASK-004`: E-Signature Dispatch & Webhook Verification
* `TASK-005`: External Signed PDF Upload & Reconstruction
* `TASK-006`: Deposit Receipt Evidence & Due Date Tracking
* `TASK-007`: Condition Waiver / Fulfillment & Countdown
* `TASK-008`: Transaction Record Sheet (TCS) Package Export
* `TASK-009`: AI Clause Assistant & Intelligent Parameterization Engine

---

## 4. How to Add New Modules (Extensibility Protocol)

As BROKO scales to additional business modules, follow this standardized onboarding workflow:

1. **Create Module Root:** Establish `<module_name>/index.md` linking to the master platform knowledge base [`index.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/index.md).
2. **Define Dedicated Skill:** Add `.agents/skills/broko-<module_name>/SKILL.md` with module-specific domain knowledge, state machines, and schemas.
3. **Initialize Task Board:** Create `<module_name>/tasks/index.md` containing numbered task files for that module's sprint deliverables.
4. **Register in Routing Matrix:** Add the new module entry to the table in Section 1 of this file and in [`index.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/index.md).
5. **Decouple Inter-Module Dependencies:** Ensure cross-module communication occurs strictly via event payloads and public service APIs (e.g. `retracct` exporting an immutable TCS packet to `accounting` via `TM-022`), never by directly mutating internal domain state.
