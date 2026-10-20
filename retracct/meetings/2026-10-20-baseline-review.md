---
title: "Meeting: 2026-10-20 Baseline Review"
date: "2026-10-20"
type: meeting
status: canonical
attendees:
  - Product Lead
  - Technical Lead
  - Broker of Record (Advisory)
tags:
  - meeting
  - baseline
  - poc
---

# Meeting Notes — 2026-10-20 Baseline Architecture Review

## Details
- **Date & Time:** October 20, 2026 — 21:00 EST
- **Attendees:** Product Lead, Technical Lead, Broker of Record (Advisory)
- **Purpose / Agenda:** Review the v3.0 master baseline, confirm Buyer Freehold POC boundaries, and align on OpenKnowledge documentation structure.

---

## Discussion & Raw Notes
1. **Scope Boundaries:**
   - Reconfirmed that commercial real estate and full trust ledger accounting remain completely out of scope.
   - Engineering effort will focus 100% on **Buyer Freehold Resale (`B01`)** as the first functional POC.
2. **Warn-Only Form Policy (`A-01`):**
   - In-depth discussion on handling unverified OREA form versions.
   - Agreed to show a persistent warning banner + notify BOR queue without hard-blocking agent drafts.
3. **Double-Representation Privacy (`Q04`):**
   - When one brokerage represents both buyer and seller, cross-party documents must be strictly firewalled. Independent penetration testing required before market release.

---

## Decisions Made
- [x] Adopt the 5-stage software and knowledge lifecycle (`Discovery -> Synthesis -> Specs -> Sprints -> QA`).
- [x] Create native OpenKnowledge templates for seamless meeting ingestion.
- [x] Baseline v3.0 documents successfully migrated into OpenKnowledge Markdown.

---

## Action Items & Impacted Artifacts

| Action Item | Owner | Target Artifact | Status |
| --- | --- | --- | --- |
| Finalize Buyer Freehold sprint breakdown | Tech Lead | [[retracct/lifecycle]] | In Progress |
| Verify licensed OREA Form 100 field mappings | Product Lead | [[retracct/05-matrices/form-and-field-registry]] | Pending |
| Schedule legal counsel review for `A-01` (Warn-Only) | BOR | [[retracct/03-compliance/ambiguities-and-resolutions]] | Pending |

---

## Open Questions / Controlled Ambiguities
- Reference: [[retracct/03-compliance/ambiguities-and-resolutions|Ambiguity A-01: Warn-only current-form status versus legally significant gates]]
- Reference: [[retracct/03-compliance/ambiguities-and-resolutions|Ambiguity A-03: FINTRAC receipt of funds holder liability]]
