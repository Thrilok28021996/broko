---
title: "Meetings & Discovery Hub"
version: "3.0"
date: "2026-10-20"
module: "Meetings"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - meetings
  - discovery
  - retracct
---

# Meetings & Discovery Hub

This directory is the ingestion landing zone for team meetings, legal reviews, stakeholder interviews, and transcripts related to the BROKO platform.

---

## Quick Actions
* **Create a New Meeting:** In OpenKnowledge desktop, click **New Document from Template** in the `retracct/meetings/` folder and choose **BROKO Meeting Note**.
* **Template Source:** `retracct/meetings/.ok/templates/meeting-note.md`

---

## Ingested Meetings Log

| Date | Title | Key Topic | Status | Impacted Artifacts |
| --- | --- | --- | --- | --- |
| 2026-10-20 | [[retracct/meetings/2026-10-20-baseline-review\|Baseline v3.0 Architecture Review]] | PRD, Buyer Freehold POC Scope, Open Decisions | Canonical | [[retracct/01-prd/master-prd]], [[retracct/05-matrices/open-decisions]] |
| 2026-10-15 | [[retracct/meetings/2026-10-15-ai-bots-module\|AI Bots Module & Clause Library Discovery]] | Clause Booklet, Parameterization Bot, Personal vs Office Library | Ingested | [[retracct/tasks/TASK-003-form-100-drafting]], [[retracct/tasks/TASK-009-ai-clause-assistant]], [[retracct/05-matrices/requirements-matrix]] |

---

## Meeting Ingestion Protocol

When adding meeting notes or pasting transcripts:
1. **Name convention:** `YYYY-MM-DD-short-slug.md` (e.g. `2026-10-21-broker-interview.md`).
2. **Tag Attendees & Date:** Record participants and precise timestamp offset.
3. **Extract Decisions:** Link confirmed decisions directly to [[retracct/05-matrices/decision-register|Decision Register]].
4. **Log Open Questions:** Flag new ambiguities in [[retracct/05-matrices/open-decisions|Open Decisions]].
5. **Assign Action Items:** Assign an owner and target document.
