---
title: "Engineering & Compliance Specification"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - engineering
  - compliance
  - apis
  - security
---

# BROKO — Engineering & Compliance Specification

> **State contracts, API events, boundary controls, audit trails, and controlled ambiguities**  
> Companion documents:
> - [[retracct/03-compliance/regulatory-guardrails|Regulatory Guardrails (TRESA / FINTRAC / OREA)]]
> - [[retracct/03-compliance/ambiguities-and-resolutions|Open Controlled Ambiguities (A-01 to A-12)]]

---

**BROKO**

**03 — Engineering & Compliance Specification**

State contracts, APIs/events, boundary controls, audit and controlled ambiguities

Final requirements baseline for design | 20 October 2026 | Ontario residential only | Not a certification of legal compliance


# 1. Service boundaries


| Service | Responsibility | Out of scope |
| --- | --- | --- |
| Transaction core | Party/property/role/transaction classification, workspace isolation, event state | CRM lead acquisition |
| Workflow + dependency | Guided/Freeflow rules, documents/tasks, evidence gates | Conferring legal validity automatically |
| Forms + clause registry | Licensed version metadata, tenant overlays, template and clause mapping | Unlicensed PDF redistribution |
| Offer register | Submission chronology, signing and counteroffers | External brokerage decision-making |
| Conditions/deadline engine | Exact rules from contract, reminders, audit, escalations | Determining contractual legal outcome for disputed facts |
| Document/signing | Generate, import, sign, store and verify evidence | Authoritative legal opinion |
| Compliance adapter | RECO/FINTRAC applicability/flags, human review and evidence | Blanket brokerage policy as law |
| Deposit tracker | Expected/received/held/released/status/evidence | Bank trust ledger and monthly reconciliation |
| TCS output | Validated package/API handoff | Commissions, payouts and accounting |
| Integrations | Form source, e-sign, calendar/email, licensed MLS, encrypted files | Unauthorized provider access |


# 2. Core domain entities

- tenant/brokerage, user, authorization role, agent-client relationship, party, confidentiality wall, property, listing, transaction, offer, offer_version, negotiation_event, signature_event, document, form_definition, form_version, clause_definition, condition, deadline_rule, deadline_event, amendment, funds_event, compliance_obligation, review_decision, audit_event, TCS_package, integration_delivery, retention_policy

# 3. Primary state machines


| Entity | States | Important guard |
| --- | --- | --- |
| Transaction | Draft → Onboarding → Listing/Offer → Negotiating → Accepted Conditional/Unconditional → Firm → Pending Closing → Closed; parallel Terminated/Failed/Archived | Only verified events change legally meaningful statuses; funds state parallel |
| Offer | Draft → Signed → Presented → Countered/Accepted/Rejected/Expired/Withdrawn | Each counteroffer is a new linked immutable version |
| Document | Draft → Prepared → Sent → Partial/Complete Signing → Verified/Review Required → Retained | Upload alone is not verified; signed original immutable |
| Condition | Identified → Active → Evidence Submitted → Fulfilled/Waived/Amended/Expired/Disputed | No automatic legal outcome on missed deadline |
| Deposit | Expected → Instructions Sent → Received/Held Elsewhere → Late/Disputed → Released/Applied | Events and proof only, not accounting ledger |
| Compliance | Not Assessed → Applicable/Exempt with rationale → Awaiting Evidence → Reviewed/Exception Escalated | Legal requirement cannot be marked satisfied by warning-only override |


# 4. API/Event integration contract examples

- POST /transactions creates or imports context; POST /transactions/{id}/events appends typed, idempotent events with evidence refs and rule version.
- POST /offers/{id}/versions records version and signer chronology; GET /offer-register?transaction_id returns role-filtered offer states.
- POST /documents/import returns pending-validation status, extraction confidence and explicit dependency matches; never auto-verified just on upload.
- POST /compliance/evaluate evaluates source-linked obligations and emits separate statutory status vs operational-warning status.
- POST /conditions/{id}/resolve accepts signed notice/waiver or amendment evidence, not free text alone when documents required.
- POST /tcs/packages composes validated read-only snapshot with source field pointers; POST /handoff sends idempotent event to separate back-office module.
- WEBHOOK /esign/{provider} validates signature and event identity before state transition; retries duplicate-safe.

# 5. Security/privacy and AI constraints

- No client documents to unapproved AI providers; approved processing must document access, retention, data locality and provider role.
- Privacy-aware document access by brokerage, role, designated representative, represented client and matter; tenant screening minimum-necessary collection, retention and human decisions.
- Signed documents and audit log tamper-evident with independent storage/version identifiers; encryption and deletion policies.
- No automatic legal assessment, release of deposits, prohibited screening or independent approval of custom clause terms.
- Retention policy defaults seven years, configurable; holds and stricter applicable legal obligations take precedence.

# 6. Source-backed compliance guardrails

- RECO TRESA guidance: multiple representation depends on brokerage vs designated model; disclosure and client consent require a distinct workflow (REG-01).
- Ontario regulation 579/05 provides the brokerage trade-record obligations and trust-account operations (REG-02); full trust ledger/monthly reconciliation implementation is explicitly delegated to separate back-office product.
- Ontario Standard Form of Lease applies to most residential tenancy agreements; OREA offer-to-lease is not an automatic substitute (REG-03).
- FINTRAC real-estate recordkeeping and identity requirements must be evaluated per actual event, role, counterparty, exception and current guidance (REG-04/05).

# 7. Open controlled ambiguities and resolution protocol


| ID | Issue | Recommended handling | Status | Risk |
| --- | --- | --- | --- | --- |
| A-01 | Q12 versus Q16 | Warn-only current-form-version status while preserving legally significant gates — Never stop navigation because latest-version check is pending, but never mislabel form as current; disclose risk, BOR notification and independently enforce actual required signing/compliance rules. | Decision chosen; legal validity of particular form needs advice | High |
| A-02 | GUIDED/FREEFLOW | Guided for common paths; Freeflow for exceptions, but earlier BOR called for both broadly — Provide guided templates to core common scenarios and freeflow selection for all permitted form/task operations, sharing controls; specialty paths may start Freeflow. | Resolved design interpretation | Medium |
| A-03 | FINTRAC receipt of funds | BOR transcript has conflicting general statements regarding holder and buyer vs seller — Base on transaction facts and current FINTRAC reporting/recordkeeping guidance, not a blanket brokerage-side rule. | Compliance mapping remains verification gate | High |
| A-04 | Form 121 vs 124 | Historical project indexes conflict about fulfillment titles — Never generate by number alone; authoritative library title/version approval needed. | Form registry task | High |
| A-05 | Lease agreement | OREA 400 offer-to-lease does not replace Ontario standard lease where required — Include standard residential lease plus exception classification and applicable disclosure/privacy controls. | Verified general source | High |
| A-06 | Residential vs commercial | User explicitly excludes commercial — Exclude commercial trade/lease workflows and commercial-specific documents from releases. | Resolved scope | Low |
| A-07 | Deposit/trust ownership | Full trust accounting excluded from current module — Track statuses/evidence/instructions, send structured events to back office; do not implement bank/trust ledger reconciliation here. | Resolved scope | Medium |
| A-08 | Specialty transactions | All designed, launch scope later — Design rule extension points and scenario briefs; do not represent as full production-validated forms at launch. | Release planning | Medium |
| A-09 | Agent signing vs legal state | Unconditional/conditional statuses are sometimes conflated with deposit and marketing sold — Track evidence-based contract state separate from listing marketing labels and funds state. | BOR transcript + legal review | High |
| A-10 | Identity document collection | BOR unsure about storage of actual government ID scans — Minimize sensitive collection; decide on lawful necessity, access, retention and method-specific evidence before production. | Privacy decision needed | High |
| A-11 | Seller/landlord listing inputs | Actual listing-side form field inventory not supplied — Final field maps depend on licensed sample files, otherwise field schema is a template pending mapping. | Licensed form library needed | High |
| A-12 | Full marketable vs first POC | Master covers all, POC builds only Buyer Freehold — Acceptance and release gate separate for proof of concept and marketed product; no blanket market-ready claim. | Resolved | Medium |


# 8. Evidence requirements for each work item

- For each rule: rule_id, version, legal/business classification, source URL, effective date, jurisdiction, applicability JSON, role, event trigger, prerequisites, completion evidence, permitted exception and downstream reevaluation.
- For each completed action: transaction_id, event_id, actor, timestamp with offset, form_version_id, immutable document hash/reference, signature evidence and reviewer determination.
- For form field mapping: form_id, version, field_id, display label, data type, required/conditional, validations, auto-prefill source, signer/party link, PDF placement and repeatable arrays.

# 9. External references

- REG-01 https://www.reco.on.ca/agents-and-brokerages/tresa-explained/tresa-faqs/
- REG-02 https://www.ontario.ca/laws/regulation/050579
- REG-03 https://www.ontario.ca/page/guide-ontarios-standard-lease
- REG-04 https://fintrac-canafe.canada.ca/guidance-directives/recordkeeping-document/record/real-eng
- REG-05 https://fintrac-canafe.canada.ca/guidance-directives/client-clientele/client/real-eng
- REG-06 https://www.orea.com/