---
title: "TASK-002: Representation & RECO Guide Delivery Gate"
date: "2026-10-20"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "Eng Team"
requirement: "[[retracct/05-matrices/requirements-matrix|TM-008, TM-009, TM-019]]"
qa_case: "[[retracct/05-matrices/qa-cases|T-004, T-016]]"
tags:
  - task
  - compliance
  - representation
  - tresa
  - poc
---

# TASK-002: Representation & RECO Guide Delivery Gate

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirements `TM-008`, `TM-009`, `TM-019` | Decisions `Q04`, `Q16`, `Q23` | QA Cases `T-004`, `T-016`

---

## 1. What This Feature Does (In Plain English)

In Ontario, before a real estate agent can officially represent a home buyer and make offers on their behalf, the law (TRESA) requires two key steps:
1. **Deliver the RECO Information Guide:** The agent must give the official provincial consumer guide to the buyer, explaining their legal rights and how commissions work.
2. **Sign the Representation Agreement (OREA Form 300):** The buyer and brokerage sign an official agreement agreeing to work together, setting the dates, commission percentage, and geographic area (e.g. Toronto).

**The Smart Gate:** To keep things fast, the system allows the agent to start drafting offer prices and terms right away. However, it **blocks the agent from sending the official offer to the seller** until the buyer has actually signed their representation agreement.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Send RECO Guide:** The agent clicks **"Send RECO Information Guide"**. The system emails the guide to the buyer and logs the exact timestamp it was sent.
2. **Prepare Form 300 (Buyer Representation):**
   * The agent sets the start date and expiry date of representation.
   * If the agreement lasts longer than 6 months, a prompt reminds the agent that the buyer must specifically initial this by law.
   * Enters the agreed commission rate (e.g., 2.5%) and the holdover period (e.g., 60 days).
   * Enters the geographic area (e.g., "City of Toronto and Region of Peel").
3. **Send for Signing:** The agent clicks **"Send Form 300 for Signature"**.
4. **Drafting Offers in the Meantime:** While waiting for the buyer to sign Form 300, the agent can immediately start writing an offer for a property.
5. **The Safety Lock:** If the agent tries to click "Submit Offer to Seller" while Form 300 is still unsigned, a pop-up warning appears:
   > *"Cannot submit offer yet: Buyer Representation Agreement (Form 300) is pending signature."*

---

## 3. Real Estate & Legal Rules (Plain English)

* **RECO Guide Must Come First:** Proof of delivery must be recorded before an offer can be presented.
* **6-Month Warning Rule:** Under Ontario law, if a representation agreement is longer than 6 months, the buyer must explicitly initial the expiry date.
* **Self-Represented Party (SRP):** If the buyer does not want representation, the system forces the agent to deliver OREA Form 320, warning the buyer that the brokerage does not represent them and owes them no loyalty.
* **Safe Drafting Allowed (`Q16`):** Agents should never be blocked from typing out an offer; only the final submission to the seller is locked.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] In a buyer deal, click "Send RECO Information Guide" and confirm the status turns green (*Delivered*).
- [ ] Create a draft Form 300 with an expiry date 7 months away and verify the system shows the 6-month initialing warning.
- [ ] While Form 300 is waiting for signature, go write an offer (Form 100) and verify the system lets you type and save it.
- [ ] Try clicking "Submit Offer to Seller" on the draft offer and verify the system blocks submission with a clear warning.
- [ ] Mark Form 300 as signed and confirm the "Submit Offer" button becomes active.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
export interface RepresentationGatePayload {
  transactionId: string;
  recoGuideDelivery: {
    deliveredAt: string; // ISO 8601
    deliveryMethod: "EMAIL" | "IN_PERSON" | "PORTAL_ACKNOWLEDGED";
    proofDocumentId?: string;
  };
  representationAgreement: {
    agreementType: "OREA_300_EXCLUSIVE_BUYER" | "OREA_310_NON_EXCLUSIVE" | "OREA_320_SELF_REPRESENTED";
    effectiveDate: string; // YYYY-MM-DD
    expiryDate: string;    // YYYY-MM-DD
    geographicScope: string;
    commissionRate: number; // e.g. 2.5
    holdoverDays: number;   // e.g. 60
    signingStatus: "DRAFT" | "SENT" | "EXECUTED";
  };
}
```

### Endpoints to Implement
* `POST /api/v1/compliance/reco-guide/deliver` — Records delivery timestamp of RECO guide.
* `POST /api/v1/agreements/representation` — Generates Form 300 payload.
* `POST /api/v1/offers/{id}/present` — Enforces gate middleware: returns `HTTP 403 Forbidden` if Form 300 is not `EXECUTED`.

### Developer Checklist
- [ ] Implement `evaluateRepresentationStatus(transactionId)` service method.
- [ ] Add 6-month date calculation validator triggering UI initialing prompt.
- [ ] Implement guard middleware on offer submission route.
- [ ] Automated tests passing for QA cases `T-004` and `T-016`.

</details>
