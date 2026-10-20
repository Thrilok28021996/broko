---
title: "Transaction Workflows & Decision Trees"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - workflows
  - state-machines
  - exceptions
---

# BROKO — Transaction Workflows & Decision Trees

> **Role-by-role workflow catalogue and exception handling baseline**  
> Companion documents:
> - [[retracct/02-workflows/buyer-workflows|Buyer Workflows (B01–B07)]]
> - [[retracct/02-workflows/seller-workflows|Seller Workflows (S01–S06)]]
> - [[retracct/02-workflows/lease-workflows|Lease Workflows (L01–L06, T01–T06)]]
> - [[retracct/02-workflows/state-machines|Interactive Mermaid State Machines]]

---

**BROKO**

**02 — Transaction Workflows & Decision Trees**

Role-by-role workflow catalogue and real-life exception handling

Final requirements baseline for design | 20 October 2026 | Ontario residential only | Not a certification of legal compliance


# 1. Unified event-driven architecture

- Transaction = brokerage-side workspace and permissions; Deal = linked offer/accepted agreement; Offer = independently versioned submission; Condition = independently clocked contractual obligation; Document = immutable signed revision chain.
- Navigation: common scenarios show Guided journey. Exception/specialty actions are Freeflow; both resolve eligibility through shared dependency rules.
- Start positions: initial client, listing, draft offer, accepted agreement or imported historical file. Import reconstructs *provisional* events until verified.
- Enforce confidential role/party separation even when one brokerage represents both sides; multiple representation/disclosure and consent are evaluated based on model and facts.

# 2. Primary workflow families


## Purchase resale

onboard → representation → property due diligence → prepare APS/schedules → validate/sign/present → counter/accept → conditions → firm → deposit/closing → record completion → TCS handoff → retain


## Sale listing

onboard seller → listing representation & listing disclosures → prepare listing records/schedules → list and manage showings/offers → register/present multiple offers → counters/acceptance → conditions → firm → deposit/closing → TCS handoff → retain


## Residential landlord

onboard landlord → listing representation → rental listing → applications/screening under privacy and human decision → negotiate/offer-to-lease → applicable standard lease → deposits/keys/occupancy handover → archive/handoff


## Residential tenant

onboard tenant → representation → search/rental application → permitted screening docs → offer-to-lease → negotiate/sign → applicable standard lease → deposits/keys/move-in → archive/handoff


## Builder / assignment / bank-owned

classify specialty transaction → request supplied agreement/schedules → compare statutory/brokerage rules → human review → signing and milestone/deposit tracking → change/closing/termination → audit and handoff


# 3. Full transaction cases


## Buyer


### B01 · Freehold resale

Onboard buyer → determine property/representation/due diligence → make offer or import agreement → clause and form package → signatures/presentation → negotiate → acceptance/conditions → firm → deposit/closing → validated handoff.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### B02 · Condominium resale

Onboard buyer → determine property/representation/due diligence → make offer or import agreement → clause and form package → signatures/presentation → negotiate → acceptance/conditions → firm → deposit/closing → validated handoff.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### B03 · POTL/common-element condo

Onboard buyer → determine property/representation/due diligence → make offer or import agreement → clause and form package → signatures/presentation → negotiate → acceptance/conditions → firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### B04 · Builder/new construction

Onboard buyer → determine property/representation/due diligence → make offer or import agreement → clause and form package → signatures/presentation → negotiate → acceptance/conditions → firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### B05 · Assignment / power of sale / bank-owned

Onboard buyer → determine property/representation/due diligence → make offer or import agreement → clause and form package → signatures/presentation → negotiate → acceptance/conditions → firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### B06 · Vacant land / rural / farm residential

Onboard buyer → determine property/representation/due diligence → make offer or import agreement → clause and form package → signatures/presentation → negotiate → acceptance/conditions → firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### B07 · Investment / tenanted residential

Onboard buyer → determine property/representation/due diligence → make offer or import agreement → clause and form package → signatures/presentation → negotiate → acceptance/conditions → firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


## Seller


### S01 · Freehold resale listing/sale

Onboard seller/listing → listing agreement and data/schedules → publish/track listing and inquiries → register competing offers → present/counter/accept → conditions → sold/firm → deposit/closing → validated handoff.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### S02 · Condominium resale listing/sale

Onboard seller/listing → listing agreement and data/schedules → publish/track listing and inquiries → register competing offers → present/counter/accept → conditions → sold/firm → deposit/closing → validated handoff.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### S03 · POTL/common-element condo

Onboard seller/listing → listing agreement and data/schedules → publish/track listing and inquiries → register competing offers → present/counter/accept → conditions → sold/firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### S04 · Assignment / bank-owned / power of sale

Onboard seller/listing → listing agreement and data/schedules → publish/track listing and inquiries → register competing offers → present/counter/accept → conditions → sold/firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### S05 · Vacant land/rural/farm residential

Onboard seller/listing → listing agreement and data/schedules → publish/track listing and inquiries → register competing offers → present/counter/accept → conditions → sold/firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### S06 · Investment / occupied residential

Onboard seller/listing → listing agreement and data/schedules → publish/track listing and inquiries → register competing offers → present/counter/accept → conditions → sold/firm → deposit/closing → validated handoff.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


## Landlord


### L01 · Condo rental

Onboard landlord → listing/property facts → receive applications with consent and privacy limits → human screening/selection → negotiate offer to lease → applicable standard lease → collect permitted deposits/evidence → occupancy handover → closeout.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### L02 · Freehold house rental

Onboard landlord → listing/property facts → receive applications with consent and privacy limits → human screening/selection → negotiate offer to lease → applicable standard lease → collect permitted deposits/evidence → occupancy handover → closeout.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### L03 · Townhouse / residential multi-unit

Onboard landlord → listing/property facts → receive applications with consent and privacy limits → human screening/selection → negotiate offer to lease → applicable standard lease → collect permitted deposits/evidence → occupancy handover → closeout.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### L04 · Secondary suite / basement apartment

Onboard landlord → listing/property facts → receive applications with consent and privacy limits → human screening/selection → negotiate offer to lease → applicable standard lease → collect permitted deposits/evidence → occupancy handover → closeout.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### L05 · Student / room rental

Onboard landlord → listing/property facts → receive applications with consent and privacy limits → human screening/selection → negotiate offer to lease → applicable standard lease → collect permitted deposits/evidence → occupancy handover → closeout.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### L06 · Lease renewal/extension/replacement

Onboard landlord → listing/property facts → receive applications with consent and privacy limits → human screening/selection → negotiate offer to lease → applicable standard lease → collect permitted deposits/evidence → occupancy handover → closeout.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


## Tenant


### T01 · Condo rental

Onboard tenant → representation and rental search → application/document requests → offer to lease and counters → applicable standard lease → deposits/keys/move in → closeout.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### T02 · House rental

Onboard tenant → representation and rental search → application/document requests → offer to lease and counters → applicable standard lease → deposits/keys/move in → closeout.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### T03 · Townhouse / multi-unit residential

Onboard tenant → representation and rental search → application/document requests → offer to lease and counters → applicable standard lease → deposits/keys/move in → closeout.

Variation trigger: configure role-, property- and agreement-specific field/form prerequisites. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### T04 · Secondary suite / basement apartment

Onboard tenant → representation and rental search → application/document requests → offer to lease and counters → applicable standard lease → deposits/keys/move in → closeout.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### T05 · Student / room rental

Onboard tenant → representation and rental search → application/document requests → offer to lease and counters → applicable standard lease → deposits/keys/move in → closeout.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


### T06 · Lease renewal / assignment / transfer

Onboard tenant → representation and rental search → application/document requests → offer to lease and counters → applicable standard lease → deposits/keys/move in → closeout.

Variation trigger: specialty agreement and additional human document review. Failed, expired, withdrawn, disputed and amended paths preserve the historical file.


# 4. Cross-cutting event and guard matrix


| Event | Guard / work item | Next action |
| --- | --- | --- |
| Client assigned | Guide/disclosure/representation applicability evaluated | Active onboarding |
| Listing authorized | Listing agreement + permissions and property data | Listing active |
| Offer received | Unique ID, source, arrival and presentation history | Registered |
| Offer countered | New signed version and irrevocability review | Negotiating |
| Offer accepted | Execution evidence + conditions extracted | Conditional or firm candidate |
| Condition fulfilled/waived | Applicable completed notice and evidence | Recompute outstanding obligations |
| Contract amended | Correct amendment, execution and conflict checks | Recompute dependent tasks/terms |
| Deposit expected/received | Event, holder, amount, due date and proof | Deposit status updated independently |
| Closing delayed/failed | Human evidence and escalation | Delayed / failed path |
| Closing confirmed | Closing evidence, remaining tasks, outcome | Closed / handoff |
| Terminated/released | Approved termination authority and deposit handling | File closeout/retain |


# 5. Special flow rules

- Same brokerage on opposite sides: party-specific permissions, screened cross-party data and explicit disclosure/consent guard before restricted activity.
- Multi-offer seller: every offer stays distinct, signed versions preserved and presentation chronology auditable.
- Document previously completed elsewhere: upload/parse/validate before dependency satisfaction; no forced re-creation.
- Form version warning: persistent banner + BOR queue, progression technically permitted, legal/compliance verification separately managed.
- Power of sale / builder: allow bank/builder-supplied agreement, specialty clauses and human applicability review; do not hard-wire OREA package.
- Residential tenancies: distinguish application/offer to lease from Ontario standard lease requirements and tenant screening privacy constraints.
- Condition counting: parse clause for calendar vs business days, holiday exclusions, date origin and exact deadline time; any uncertainty escalates.