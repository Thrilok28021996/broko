---
title: "BROKO — Developer Implementation Guide & Architecture Handbook"
version: "3.0"
date: "2026-10-22"
module: "Engineering"
parent: "[[retracct/index|BROKO Index]]"
tags:
  - developer-guide
  - architecture
  - engineering
  - implementation
  - prisma
  - api-specs
---

# BROKO — Developer Implementation Guide & Architecture Handbook

> **Target Audience:** Software Engineers, Backend/Frontend Developers, AI Coding Agents  
> **Source Requirements:** Broker of Record (BOR) & Ontario Domain Expert Specifications (RECO TRESA / OREA / FINTRAC)  
> **Target Deliverable:** **Sprint 1 — Buyer Freehold Resale POC (`B01`)**

---

## 1. Executive Mission & System Purpose

This handbook translates the Broker of Record (BOR) and Ontario real estate domain expert requirements into a concrete, developer-friendly software architecture. 

As a developer building **`retracct`**, your objective is to implement an event-driven transaction engine for Ontario residential real estate that enforces statutory compliance without sacrificing agent velocity.

---

## 2. Standard Technology Architecture

To maintain consistency across all developers and AI assistants, build the module using the following stack:

| Component | Standard Technology | Developer Responsibility |
| :--- | :--- | :--- |
| **Backend Runtime** | Node.js (v20+) with TypeScript | Strict type checking (`strict: true`), zero `any` types. |
| **Framework** | Fastify or Express / NestJS | RESTful API controllers, Zod/Joi schema validation pipes. |
| **Database & ORM** | PostgreSQL 16+ with Prisma ORM | Relational data integrity, ACID transactions, Row-Level Security (RLS). |
| **Caching & Queues** | Redis with BullMQ | Idempotent webhook locks, countdown scheduler, background PDF jobs. |
| **Object Storage** | S3 / GCS compatible private vault | Encrypted PDF storage, immutable versioning, SHA-256 checksums. |
| **PDF Manipulation** | `pdf-lib` / `pdfjs-dist` | Form 100 tab stamping, split-pane OCR bounding boxes, PDF bundle merger. |
| **Frontend UI** | Next.js 14+ (App Router) / React | Tailwind CSS, responsive mobile layout, real-time countdown widgets. |

---

## 3. Recommended Codebase Directory Layout

Organize your backend service implementation as follows:

```
src/
├── core/
│   ├── domain/                         <-- Core entities & value objects
│   │   ├── Transaction.ts
│   │   ├── Party.ts
│   │   ├── Offer.ts
│   │   ├── Condition.ts
│   │   └── Deposit.ts
│   ├── events/                         <-- Typed immutable domain events
│   │   ├── DomainEvent.ts
│   │   ├── TransactionCreatedEvent.ts
│   │   └── ConditionFulfilledEvent.ts
│   └── errors/                         <-- Domain invariant error classes
│       ├── DomainError.ts
│       ├── TRESAGateViolationError.ts
│       └── FinancialBalanceMismatchError.ts
│
├── modules/
│   ├── onboarding/                     <-- TASK-001: Party & Role Classification
│   ├── representation/                 <-- TASK-002: RECO Guide & Form 300 Gate
│   ├── offer-drafting/                 <-- TASK-003: Form 100, Schedule A, Counteroffers
│   ├── esign/                          <-- TASK-004: E-Sign Envelopes & Webhook Adapter
│   ├── document-import/                <-- TASK-005: External PDF & Email Forwarding Gateway
│   ├── deposits/                       <-- TASK-006: 24h Countdown & Wire Evidence
│   ├── conditions/                     <-- TASK-007: Ontario Holiday/DST Countdown Engine
│   ├── tcs/                            <-- TASK-008: Conveyancing Package Compilation
│   └── clause-assistant/               <-- TASK-009: 10 Standard Clauses & Diff Summarizer
│
├── infrastructure/
│   ├── database/                       <-- Prisma client & schema
│   ├── email/                          <-- Inbound email parser (offers@broko.ai)
│   ├── storage/                        <-- Encrypted S3 vault client
│   └── calendar/                       <-- Ontario statutory holiday service
│
└── api/
    ├── routes/                         <-- Fastify/Express route declarations
    ├── controllers/                    <-- HTTP request handlers
    └── middlewares/                    <-- Auth, Firewall & Idempotency guards
```

---

## 4. The 9 BOR Domain Invariants (Non-Negotiable Rules)

Every developer building endpoints or services must enforce these 9 statutory/domain rules:

1. **Confidentiality Firewall Invariant (`Q04`, `T-004`):**  
   When the brokerage represents both buyer and seller in-house, buyer agents and seller agents must have separate, cryptographically isolated workspaces. A buyer agent cannot see seller reserve notes, and a seller agent cannot see buyer drafts.
2. **RECO Guide Gate Invariant (`TM-009`, `Q16`):**  
   An agent may draft offers while representation is pending, but `POST /offers/{id}/present` is **hard-blocked (HTTP 403)** unless verified timestamps exist for RECO Information Guide delivery and Form 300/320 execution.
3. **Financial Math Invariant (`TASK-003`):**  
   `PurchasePrice === DepositAmount + AdditionalDepositAmount + BalanceOnClosing`. If this equation does not balance, reject with **HTTP 422**.
4. **Counteroffer Session Continuity Invariant (`Q09`, `TASK-003`):**  
   Counteroffers do NOT create detached transactions. They are modeled as sequential versions (`v1`, `v2`, `v3`) of the same offer entity, preserving historical strike-outs, initials, and flips of irrevocability.
5. **Confirmation of Acceptance (COA) Invariant (`TASK-003`):**  
   An offer does not transition to `ACCEPTED_CONDITIONAL` or `ACCEPTED_FIRM` until the **Confirmation of Acceptance (COA)** block is executed by the final accepting party. Delivery Acknowledgement occurs immediately thereafter.
6. **Human-in-the-Loop AI Invariant (`Q14`, `TASK-009`):**  
   The AI Clause Assistant generates proposed text only. Direct insertion into Schedule A is forbidden without explicit human agent click-to-approve.
7. **No Silent Legal Forfeiture Invariant (`Q19`, `TASK-007`):**  
   When a condition deadline expires without signed fulfillment (OREA 124) or waiver (OREA 123), the engine transitions status to `EXPIRED_UNRESOLVED` and notifies humans. The system **never** unilaterally terminates the contract.
8. **Deposit Decoupling Invariant (`Q20`, `TASK-006`):**  
   An unconditional deal transitions to `FIRM` independently of deposit proof. An unreceived deposit raises high-priority BOR alerts without cancelling the contract.
9. **Conveyancing Export Invariant (`TM-022`, `TASK-008`):**  
   `POST /tcs/export` is hard-blocked (HTTP 400) if mandatory conveyancing fields (buyer legal name, legal description, deposit verification, lawyer contacts) are absent.

---

## 5. Master Prisma Database Schema

Place this consolidated schema in `prisma/schema.prisma`:

```prisma
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

generator client {
  provider = "prisma-client-js"
}

enum TransactionType {
  BUYER_RESIDENTIAL
  SELLER_RESIDENTIAL
  LANDLORD_RESIDENTIAL
  TENANT_RESIDENTIAL
}

enum PropertySubtype {
  FREEHOLD_RESALE
  CONDOMINIUM_RESALE
  POTL
  BUILDER_ASSIGNMENT
}

enum RepresentationType {
  DESIGNATED_REPRESENTATION
  BROKERAGE_REPRESENTATION
  SELF_REPRESENTED
}

enum TransactionStatus {
  DRAFT
  ONBOARDING
  OFFER_PREPARATION
  OFFER_PRESENTED
  NEGOTIATING_COUNTER
  ACCEPTED_CONDITIONAL
  FIRM
  PENDING_CLOSING
  CLOSED
  TERMINATED
}

enum DepositStatus {
  EXPECTED
  INSTRUCTIONS_SENT
  RECEIVED_CONFIRMED
  OVERDUE
  DISPUTED
}

enum ConditionStatus {
  ACTIVE
  EVIDENCE_SUBMITTED
  FULFILLED
  WAIVED
  EXPIRED_UNRESOLVED
  TERMINATED
}

model Transaction {
  id                 String             @id @default(uuid())
  brokerageId        String
  designatedAgentId  String
  type               TransactionType    @default(BUYER_RESIDENTIAL)
  propertySubtype    PropertySubtype    @default(FREEHOLD_RESALE)
  representationType RepresentationType @default(DESIGNATED_REPRESENTATION)
  status             TransactionStatus  @default(ONBOARDING)
  createdAt          DateTime           @default(now())
  updatedAt          DateTime           @updatedAt

  parties            Party[]
  offers             Offer[]
  conditions         Condition[]
  deposits           Deposit[]
  documents          Document[]
  auditEvents        AuditEvent[]
}

model Party {
  id             String      @id @default(uuid())
  transactionId  String
  role           String      // PRIMARY_BUYER, CO_BUYER, SELLER
  legalFullName  String
  email          String
  phone          String
  streetAddress  String
  city           String
  province       String      @default("ON")
  postalCode     String
  fintracVerified Boolean    @default(false)

  transaction    Transaction @relation(fields: [transactionId], references: [id])
}

model Offer {
  id                     String         @id @default(uuid())
  transactionId          String
  versionNumber          Int            @default(1)
  isCounterOffer         Boolean        @default(false)
  parentOfferId          String?
  purchasePrice          Decimal        @db.Decimal(12, 2)
  depositAmount          Decimal        @db.Decimal(12, 2)
  balanceDueOnClosing    Decimal        @db.Decimal(12, 2)
  irrevocableDeadline    DateTime
  completionDate         DateTime
  requisitionDate        DateTime
  chattelsIncluded       String[]
  fixturesExcluded       String[]
  rentalItems            String[]
  confirmationOfAcceptanceAt DateTime?
  acknowledgedAt         DateTime?
  status                 String         // DRAFT, SIGNED, PRESENTED, COUNTERED, ACCEPTED, REJECTED

  transaction            Transaction    @relation(fields: [transactionId], references: [id])
  scheduleAConditions    Condition[]
}

model Condition {
  id                  String          @id @default(uuid())
  transactionId       String
  offerId             String?
  clauseType          String          // MORTGAGE, INSPECTION, LAWYER_REVIEW, ACCESS, INSURANCE
  libraryScope        String          @default("STANDARD_OREA") // STANDARD_OREA, OFFICE, PERSONAL
  clauseText          String          @db.Text
  parametersJson      Json
  irrevocableDeadline DateTime
  status              ConditionStatus @default(ACTIVE)
  resolutionDocId     String?
  resolvedAt          DateTime?

  transaction         Transaction     @relation(fields: [transactionId], references: [id])
  offer               Offer?          @relation(fields: [offerId], references: [id])
}

model Deposit {
  id                  String        @id @default(uuid())
  transactionId       String
  amount              Decimal       @db.Decimal(12, 2)
  payableTiming       String        // HEREWITH, UPON_ACCEPTANCE, AS_OTHERWISE_DESCRIBED
  dueTimestamp        DateTime
  holderClassification String       // LISTING_BROKERAGE, CO_OPERATING_BROKERAGE, LAWYER
  holderName          String
  status              DepositStatus @default(EXPECTED)
  receiptDocumentId   String?
  paymentReference    String?
  confirmedAt         DateTime?

  transaction         Transaction   @relation(fields: [transactionId], references: [id])
}

model Document {
  id             String      @id @default(uuid())
  transactionId  String
  documentType   String      // RECO_GUIDE, OREA_300, OREA_100, OREA_124, DEPOSIT_PROOF
  title          String
  s3Url          String
  sha256Checksum String
  signedAt       DateTime?
  isVerified     Boolean     @default(false)
  uploadedAt     DateTime    @default(now())

  transaction    Transaction @relation(fields: [transactionId], references: [id])
}

model AuditEvent {
  id             String      @id @default(uuid())
  transactionId  String
  eventType      String      // TRANSACTION_CREATED, OFFER_PRESENTED, CONDITION_FULFILLED
  payloadJson    Json
  actorUserId    String
  timestamp      DateTime    @default(now())

  transaction    Transaction @relation(fields: [transactionId], references: [id])
}
```

---

## 6. Granular API Specifications for Sprint 1

| Endpoint | Method | Input Schema | Success Status | Invariant / Guardrail |
| :--- | :---: | :--- | :---: | :--- |
| `/api/v1/transactions` | `POST` | `BuyerOnboardingPayload` | `201 Created` | Enforce Ontario address and representation model (`TASK-001`). |
| `/api/v1/compliance/reco-guide` | `POST` | `{ transactionId, deliveryTimestamp }` | `200 OK` | Capture mandatory delivery timestamp before Form 300 (`TASK-002`). |
| `/api/v1/offers` | `POST` | `Form100OfferPayload` | `201 Created` | Enforce `Price = Deposit + Balance`; check holiday/DST (`TASK-003`). |
| `/api/v1/offers/{id}/present` | `POST` | `{ presentedToAgentEmail, method }` | `200 OK` | Hard-gated: HTTP 403 if RECO guide or Form 300 is unexecuted (`TASK-002`). |
| `/api/v1/integrations/esign/dispatch` | `POST` | `ESignEnvelopeDispatchPayload` | `202 Accepted` | Mapped signature tabs; sequential or parallel order (`TASK-004`). |
| `/api/v1/integrations/esign/webhook` | `POST` | `ESignWebhookEventPayload` | `200 OK` | HMAC verified; redis idempotency lock; download certificate (`TASK-004`). |
| `/api/v1/documents/import` | `POST` | Multipart PDF | `201 Created` | Computes SHA-256; marks `PENDING_VERIFICATION` (`TASK-005`). |
| `/api/v1/inbound-email/gateway` | `POST` | Inbound Email Webhook payload | `200 OK` | Matches agent email; extracts PDF attachment; stages offer (`TASK-005`). |
| `/api/v1/deposits/{id}/receipt` | `POST` | `{ receiptDocId, referenceNo }` | `200 OK` | Decoupled from firm status; clears overdue escalation (`TASK-006`). |
| `/api/v1/conditions/{id}/resolve` | `POST` | `ConditionResolutionPayload` | `200 OK` | Requires OREA 124 or 123 PDF; auto-firms when all fulfilled (`TASK-007`). |
| `/api/v1/transactions/{id}/tcs/export` | `POST` | Empty | `200 OK` | Hard-gated: HTTP 400 if deal is not FIRM or fields missing (`TASK-008`). |
| `/api/v1/clauses/prompt-session` | `POST` | `{ agentIntentInput }` | `200 OK` | AI prompts variables; staging modal; PII blocked (`TASK-009`). |
| `/api/v1/offers/{id}/diff` | `GET` | Empty | `200 OK` | Returns AI difference summary between counteroffer rounds (`TASK-009`). |

---

## 7. Developer Test Strategy & QA Verification

Every feature implementation must be accompanied by automated tests satisfying the corresponding test case in [`retracct/05-matrices/qa-cases.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/05-matrices/qa-cases.md):

```bash
# Run unit and integration suite
npm test

# Run statutory holiday & deadline countdown tests (T-008)
npm test -- test/conditions/ontario-calendar.test.ts

# Run dual-representation firewall penetration tests (T-004)
npm test -- test/security/firewall.test.ts

# Run e-sign webhook idempotency tests (T-020)
npm test -- test/integrations/webhook-idempotency.test.ts
```

For detailed specifications on each individual task, refer directly to [`retracct/tasks/`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/retracct/tasks/).
