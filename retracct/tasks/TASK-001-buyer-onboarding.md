---
title: "TASK-001: Buyer Onboarding & Party Classification"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-001, TM-002]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-001, T-004]]"
tags:
  - task
  - onboarding
  - core
  - poc
---

# TASK-001: Buyer Onboarding & Party Classification

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-001`, `TM-002` | Decisions `Q01`, `Q04`, `Q05` | QA Cases `T-001`, `T-004`

---

## 1. Technical Objective & User Context

### Technical Objective (What Developers Build)
Build the transaction initialization service and REST API endpoint (`POST /api/v1/transactions`) to register residential buyer transactions, store legal party profiles, and enforce Row-Level Security (RLS) database firewalls between buyer and seller workspaces.

### End-User Persona
* **Primary User:** Licensed Ontario Real Estate Agent (Buyer's Realtor)
* **Supervisory User:** Broker of Record (BOR) / Compliance Administrator

### Business Rationale & Domain Context
Under Ontario's Trust in Real Estate Services Act (TRESA), real estate brokerages representing buyers must establish client representation models and legal party identities upfront. In-house transactions where the same brokerage represents both parties require cryptographic workspace isolation to prevent confidentiality breaches (`T-004`).

---

## 2. Technical Data Contract & Schema

```typescript
export interface BuyerOnboardingPayload {
  brokerageId: string; // UUID
  agentId: string;     // UUID of designated agent
  transactionType: "BUYER_RESIDENTIAL";
  propertySubtype: "FREEHOLD_RESALE"; // B01 scenario
  representationType: "DESIGNATED_REPRESENTATION" | "BROKERAGE_REPRESENTATION" | "SELF_REPRESENTED";
  parties: Array<{
    partyId: string;
    role: "PRIMARY_BUYER" | "CO_BUYER" | "GUARANTOR";
    legalFullName: string;
    email: string;
    phone: string;
    residenceAddress: {
      street: string;
      city: string;
      province: "ON";
      postalCode: string;
    };
    fintracVerified: boolean;
  }>;
  targetProperty?: {
    streetAddress: string;
    municipality: string;
    postalCode: string;
    mlsNumber?: string;
    legalDescription?: string;
  };
}
```

---

## 3. Core Acceptance Criteria

- [ ] **Transaction Initialization:** Creates a transaction entity in `ONBOARDING` state with an immutable UUID, brokerage ID, and timestamp.
- [ ] **Property Classification:** Sets property class to `Residential Freehold Resale` (`B01`), locking in OREA Form 100 as the required offer template.
- [ ] **Party Hierarchy:** Supports multiple buyers (primary buyer, spouses, co-signers) with full legal names matching government-issued ID.
- [ ] **Confidentiality Firewalling (`Q04`):**
  - Isolates buyer workspace permissions from any potential in-house listing or counterparty agent.
  - Generates independent cryptographic tenant keys preventing cross-party document leakage (`T-004`).
- [ ] **Event Emission:** Appends immutable audit events to the transaction log:
  - `TRANSACTION_CREATED` (payload: metadata, timestamp, creatorId)
  - `PARTY_REGISTERED` (payload: partyId, role, legalFullName)
- [ ] **Validation Guardrails:**
  - Reject transactions missing valid Ontario postal codes or contact emails.
  - Require explicit selection of representation model before progressing to offer drafting.

---

## 4. Implementation Steps

1. **Database / Entity Layer:**
   - Define PostgreSQL table schemas for `transactions`, `parties`, and `agent_client_relationships`.
   - Add foreign key constraints and row-level security (RLS) policies scoped by `brokerageId` and `agentId`.
2. **API Endpoint (`POST /api/v1/transactions`):**
   - Implement controller and validation pipe with Zod/Joi schema.
   - Return HTTP 201 with populated `transactionId` and initial state `ONBOARDING`.
3. **Event Stream Integration:**
   - Publish `TRANSACTION_CREATED` event to transactional audit log.
4. **Front-End Guided Onboarding Flow:**
   - Step 1: Client details (Name, Email, Phone, Address).
   - Step 2: Property type selection (Default: Freehold Resale).
   - Step 3: Representation choice (Designated vs Brokerage vs Self-Represented).

---

## 5. Verification & QA Test Links

- [ ] `T-001`: Verify transaction creation and party provenance.
- [ ] `T-004`: Dual-representation firewall test — verify in-house listing agent cannot read buyer notes or drafted terms.
