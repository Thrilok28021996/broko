---
title: "Interactive State Machines & Domain Lifecycles"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/02-workflows/workflow-catalogue|Workflow Catalogue]]"
tags:
  - state-machines
  - mermaid
  - architecture
---

# Interactive State Machines & Domain Lifecycles

BROKO's domain is driven by six core state machines operating with strict event validation.

---

## 1. Transaction State Machine

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Onboarding: Client Assigned
    Onboarding --> ListingOrOffer: Rep Agreement Executed
    ListingOrOffer --> Negotiating: Offer Created / Received
    Negotiating --> AcceptedConditional: Conditional Acceptance
    Negotiating --> AcceptedUnconditional: Unconditional Acceptance
    AcceptedConditional --> Firm: All Conditions Satisfied / Waived
    AcceptedUnconditional --> Firm: Deposit Confirmed
    Firm --> PendingClosing: Documents Verified for Conveyance
    PendingClosing --> Closed: Closing Handoff Confirmed
    
    Negotiating --> Terminated: All Offers Expired / Rejected
    AcceptedConditional --> Terminated: Condition Failed / Mutual Release
    Closed --> [*]
    Terminated --> [*]
```

---

## 2. Offer & Counteroffer State Machine

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Signed: All Parties E-Signed
    Signed --> Presented: Delivery Recorded to Counterparty
    Presented --> Accepted: Signed Without Alteration
    Presented --> Countered: Altered & Sent Back
    Presented --> Rejected: Explicit Rejection Recorded
    Presented --> Expired: Irrevocable Time Passed
    Presented --> Withdrawn: Revocation Before Acceptance
    
    Countered --> [*]: Spawns Linked New Version
    Accepted --> [*]
    Rejected --> [*]
    Expired --> [*]
    Withdrawn --> [*]
```

---

## 3. Condition Lifecycle State Machine

```mermaid
stateDiagram-v2
    [*] --> Identified
    Identified --> Active: Incorporated in Executed Agreement
    Active --> EvidenceSubmitted: Notice / Waiver Uploaded
    EvidenceSubmitted --> Fulfilled: True Condition Precedent Met
    EvidenceSubmitted --> Waived: Unilateral Waiver Exercised
    Active --> Amended: Deadline Extended via Amendment
    Active --> Expired: Irrevocable Deadline Reached Without Notice
    Active --> Disputed: Conflicting Notice / Release Contested
    
    Fulfilled --> [*]
    Waived --> [*]
    Expired --> [*]
```

---

## 4. Document Verification State Machine

```mermaid
stateDiagram-v2
    [*] --> Draft
    Draft --> Prepared: Fields & Signers Bound
    Prepared --> Sent: Dispatched to Signers
    Sent --> PartiallySigned: Some Signatures Collected
    PartiallySigned --> CompleteSigning: All Signatures Complete
    CompleteSigning --> UnderReview: Uploaded / Imported for Verification
    UnderReview --> Verified: Broker / Compliance Approval
    UnderReview --> Rejected: Deficiencies Identified
    Verified --> Retained: 7-Year Compliance Archive
    
    Rejected --> Draft: Re-preparation Required
    Retained --> [*]
```

---

## 5. Deposit Lifecycle State Machine

```mermaid
stateDiagram-v2
    [*] --> Expected
    Expected --> InstructionsSent: Wire / Draft Details Dispatched
    InstructionsSent --> Received: Proof of Deposit Submitted
    Received --> VerifiedByBrokerage: Ledger / Receipt Matched
    Expected --> Late: Due Date Exceeded
    Received --> Disputed: Contested by Counterparty
    VerifiedByBrokerage --> Released: Closing Application or Mutual Release
    Released --> [*]
```

---
*See [[retracct/03-compliance/engineering-compliance|Engineering Compliance]] for API event contracts driving these states.*
