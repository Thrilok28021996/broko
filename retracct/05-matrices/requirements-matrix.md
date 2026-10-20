---
title: "Functional Requirements Matrix (TM-001–TM-030)"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - matrix
  - requirements
---

# Functional Requirements Matrix (TM-001–TM-030)

| ID | Module | Detailed behavior | Priority | Decision references |
| --- | --- | --- | --- | --- |
| TM-001 | Core | Create/link transaction from onboarding, listing, offer, accepted deal or import; preserve source provenance. | P0 | Q05,Q07 |
| TM-002 | Core | Classify role, property type, transaction family, representation, parties, source and specialty flags. | P0 | Q01,Q04 |
| TM-003 | Workflow | Guided mode for common workflows, Freeflow for exceptions; one shared rule/dependency engine; neither mode bypasses substantive checks. | P0 | Q08,Q16 |
| TM-004 | Workflow | Import externally completed documents, reconstruct events and validate evidence before satisfying dependencies. | P0 | Q07 |
| TM-005 | Forms | Hybrid master plus brokerage-specific approved form library; capture purpose, trigger, version, source, applicability, dependencies and signature map. | P0 | Q11,Q13 |
| TM-006 | Forms | If latest version unverified, display persistent warning, alert BOR and allow navigation/action; do not label as verified or automatically attest compliance. | P0 | Q12 |
| TM-007 | Forms | Allow form creation/upload and digital field mapping; validate required data, signatures, role and transaction association. | P0 | Q11,Q13 |
| TM-008 | Representation | Support designated/brokerage representation, self-represented parties and multiple representation, including independent confidential client workspaces. | P0 | Q04,Q23 |
| TM-009 | Disclosure | Track RECO Information Guide and representation disclosure timing; missing statutory evidence remains unresolved even where workflow continues. | P0 | Q16,Q23 |
| TM-010 | Offers | Multiple offer register: unique offer IDs, received/presented timestamps, agent, version, presentation evidence, outcome and status per offer. | P0 | Q09 |
| TM-011 | Offers | Negotiation: counteroffers, expiry/irrevocability, rejection, withdrawal, revised offers, multi-offer integrity and audit history. | P0 | Q09 |
| TM-012 | Clauses | Select broker-approved clauses, fill variables and allow custom AI draft proposals only with human review. | P0 | Q13,Q14 |
| TM-013 | Conditions | Each condition is an obligation with exact source text, due rule/time, party, evidence, waiver/fulfillment/amendment, alerts and resolution. | P0 | Q18,Q19 |
| TM-014 | Amendments | After execution, contractual changes use transaction-appropriate amendment/versioning, with required signatures and downstream impact analysis. | P0 | Q15 |
| TM-015 | Signatures | E-sign provider, signed upload and standalone signing fallback; capture audit proof and signing failures. | P0 | Q17 |
| TM-016 | Transactions | Manage acceptance, conditional, firm, pending closing, delayed closing, failed closing, terminated, closed and retained separately. | P0 | Q21 |
| TM-017 | Deposits | Track expected/received/held/released statuses, due events, instructions, receipt evidence and escalation, without doing full trust ledger reconciliation. | P0 | Q20 |
| TM-018 | Leases | Landlord/tenant listings, applications, privacy-controlled screening, human decisions, offer-to-lease and applicable Ontario standard lease completion. | P0 | Q10 |
| TM-019 | Compliance | Source-linked RECO/FINTRAC/brokerage rules with reasoned applicability, evidence, overrides and BOR/delegated review. | P0 | Q23,Q25 |
| TM-020 | Security | No transfer of private documents to unapproved AI models; access control, audit, encryption and record-specific permissions. | P0 | Q26 |
| TM-021 | Retention | Seven-year default configurable with retention starts, category overrides, holds and controlled deletion. | P1 | Q24 |
| TM-022 | Handoff | Produce agent-validated deal submission/TCS packet and structured event payload; accounting/trust-ledger processing outside scope. | P0 | Q22 |
| TM-023 | Integrations | Adapters for authorized form source, e-sign, email/calendar, licensed property/MLS data and document storage; idempotent events and failure recovery. | P1 | Q27 |
| TM-024 | Experience | Agent/reviewer task dashboards, mobile-responsive transaction views, status, next action, reminders and accessibility. | P0 | Q28,Q29 |
| TM-025 | Listing | Seller/landlord listing workflows with listing documents, amendments, price changes, offers, withdrawal/suspension and relisting. | P0 | Q06 |
| TM-026 | Governance | Configure activity-level gates independently: warn-only form-version control vs legally consequential review requirement. | P0 | Q12,Q16 |
| TM-027 | Scenarios | Maintain configurable property/transaction/role matrix including specialty scenarios without asserting automatic same-form applicability. | P1 | Q01,Q03 |
| TM-028 | Dual-sided | Support in-house opposite sides, conflicts, designated representatives, disclosure/consent and restricted cross-party visibility. | P0 | Q04 |
| TM-029 | Audit | Immutable event log for offer presentation, signing, condition resolution, amendment, status, exception and evidence decisions. | P0 | Q09,Q19 |
| TM-030 | Operations | Operational observability: job failures, retries, deadlines, documents awaiting review and regulatory warning dashboard. | P1 | Q28 |
