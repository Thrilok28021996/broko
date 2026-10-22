---
title: "BROKO — Offer & Transaction Management"
version: "3.0"
date: "2026-10-20"
module: "Transaction & Offer Management"
parent: "[[index|BROKO Master Index]]"
status: "Baseline"
jurisdiction: "Ontario (Residential)"
tags:
  - real-estate
  - ontario
  - orea
  - tresa
  - transaction-management
  - broko
---

# BROKO — Offer & Transaction Management

> **Baseline Version 3.0** | Date: October 20, 2026  
> **Jurisdiction:** Ontario Residential Transactions (OREA / RECO TRESA)  
> **Target Scope:** Complete Transaction Management Module (Buyer Freehold POC First)

> [!IMPORTANT]
> **Source Fidelity & Legal Disclaimer:**  
> Field mappings for every licensed OREA form require authorized, current PDF versions. Source-backed legal rules require go-live legal review. Warn-only current-version status does not constitute legal compliance.

---

## Executive Overview

BROKO is a transaction and offer management platform engineered specifically for the Ontario residential real estate market under the Trust in Real Estate Services Act (TRESA). It provides an event-driven, audit-grade workflow engine supporting all four major residential roles: **Buyer**, **Seller**, **Landlord**, and **Tenant**.

### Core Boundaries
- **In Scope:** Ontario residential transactions across Freehold, Condominium, POTL, Builder/Assignment, and Tenancy scenarios.
- **Engineering POC Scope:** **Buyer Freehold Resale** (`B01`) end-to-end (onboarding, representation, Form 100 offer drafting, signing, deposit tracking, condition fulfillment, and transaction record sheet export).
- **Explicit Exclusions:** Brokerage general CRM, trust ledger accounting / fund disbursements, commission payout calculations, and commercial real estate.

---

## Knowledge Base Navigation

### [[retracct/lifecycle|00. Software & Knowledge Lifecycle]]
End-to-end framework connecting discovery, meetings, specifications, sprint delivery, and release gates:
- [[retracct/lifecycle|Software Lifecycle Flowchart & Stage Gates]]
- [[retracct/DEVELOPER-GUIDE|Developer Implementation Guide & Architecture Handbook]]
- [[retracct/meetings/index|Meetings & Discovery Ingestion Hub]] (*with built-in meeting template*)
- [[retracct/tasks/index|Task Management & Sprint Board]] (*Sprint 1 POC tracking*)

### [[retracct/01-prd/master-prd|01. Master PRD]]
Comprehensive product requirements document establishing:
- Executive contracts and operational boundaries
- The 5 operating personas and granular permissions
- Transaction taxonomy across Buyer, Seller, Landlord, and Tenant
- Numbered functional requirements (`TM-001` through `TM-030`)
- [[retracct/01-prd/decisions-reconciled|30 Reconciled Architectural Decisions (Q01–Q30)]]

### [[retracct/02-workflows/workflow-catalogue|02. Workflows & State Machines]]
End-to-end execution journeys and state transition logic:
- [[retracct/02-workflows/workflow-catalogue|Unified Event-Driven Architecture & Guard Matrix]]
- [[retracct/02-workflows/buyer-workflows|Buyer Workflows (B01–B07)]] — *Includes Primary Buyer Freehold POC*
- [[retracct/02-workflows/seller-workflows|Seller Workflows (S01–S06)]] — Listing-to-completion, counteroffer registers
- [[retracct/02-workflows/lease-workflows|Lease Workflows (L01–L06, T01–T06)]] — Ontario Standard Lease compliance
- [[retracct/02-workflows/state-machines|Interactive Mermaid State Machines]] — Transaction, Offer, Document, Condition, Deposit & Compliance

### [[retracct/03-compliance/engineering-compliance|03. Engineering & Compliance Specification]]
Technical architecture and regulatory guardrails:
- [[retracct/03-compliance/engineering-compliance|Service Boundaries & Domain Entities]]
- API and Event contracts (`POST /transactions`, `POST /offers`, `WEBHOOK /esign`)
- [[retracct/03-compliance/regulatory-guardrails|Source-Backed Compliance Guardrails (TRESA, FINTRAC, OREA)]]
- [[retracct/03-compliance/ambiguities-and-resolutions|Open Controlled Ambiguities (A-01 to A-12)]]

### [[retracct/04-qa-release/poc-release-plan|04. Release Plan & QA Strategy]]
Delivery roadmap, test plans, and market-readiness verification:
- [[retracct/04-qa-release/poc-release-plan|Buyer Freehold POC Scope & Delivery Gates]]
- [[retracct/04-qa-release/qa-test-plan|QA Strategy & Environment Matrix]]
- Market-Release Checklist & Broker of Record (BOR) sign-off criteria

### [[retracct/05-matrices/decision-register|05. Traceability & Reference Matrices]]
Authoritative cross-linked registers extracted from the master workbook:
- [[retracct/05-matrices/decision-register|Decision Register (Q01–Q30)]]
- [[retracct/05-matrices/scenario-catalogue|Scenario Catalogue (25 Scenarios)]]
- [[retracct/05-matrices/requirements-matrix|Functional Requirements Matrix (TM-001–TM-030)]]
- [[retracct/05-matrices/form-and-field-registry|Form & Field Registry (17 Standard Records)]]
- [[retracct/05-matrices/open-decisions|Open Decisions Register (A-01–A-12)]]
- [[retracct/05-matrices/qa-cases|QA Cases (T-001–T-026)]]
- [[retracct/05-matrices/regulatory-sources|Regulatory Sources (REG-01–REG-06)]]
- [[retracct/05-matrices/field-dictionary|Domain Field Dictionary Starter]]

---

## Baseline Source Files
Original binary source documents are archived in [`retracct/sources/`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/sources) for reference verification.
