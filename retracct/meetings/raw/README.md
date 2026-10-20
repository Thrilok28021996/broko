---
title: "Raw Transcripts Repository"
version: "3.0"
date: "2026-10-20"
module: "Meetings"
parent: "[[retracct/meetings/index|Meetings Hub]]"
tags:
  - transcripts
  - raw-data
---

# Raw Transcripts Repository

Drop unedited, raw transcript files here (`.txt`, `.vtt`, `.srt`, `.json`, `.md`).

---

## How Ingestion Works

1. **Drop File:** Save your raw transcript file in this folder (e.g., `2026-10-21-broker-call.vtt` or `.txt`).
2. **Trigger Ingestion:** Tell Antigravity:
   > *"Ingest the transcript in `retracct/meetings/raw/2026-10-21-broker-call.txt`"*
3. **Automated Processing:**
   - A structured meeting doc will be created in [`retracct/meetings/`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/meetings).
   - An executive summary, decisions, and action items will be extracted automatically.
   - Any new architectural ambiguities will be mapped to [[retracct/05-matrices/open-decisions|Open Decisions]].
   - Action items will be routed to the respective PRD, Workflow, or QA matrices.
   - The verbatim transcript will be preserved in full for auditability.
