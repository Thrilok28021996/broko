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

## 1. What This Feature Does (In Plain English)

When a real estate agent takes on a new home buyer, they need a dedicated, private file in the system to manage the deal.

This task creates the **Buyer Onboarding** flow. It allows the agent to:
1. Enter the buyer's full legal details (name, email, phone, home address).
2. Add spouses or co-buyers who are buying the property together.
3. Set the property type to **Freehold House (Resale)**.
4. Ensure total privacy: if another agent in the same brokerage is representing the seller, that seller's agent cannot peek into this buyer's private notes or offer amounts.

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Click New Deal:** The agent clicks **"+ New Transaction"** on their dashboard and selects **"Buyer"**.
2. **Select Property Type:** The agent chooses **"Residential Freehold Resale"** (standard standalone house or townhouse).
3. **Enter Buyer Details:**
   * Enters the primary buyer's legal full name (must match their driver's license or passport).
   * Enters their email, phone number, and current home address.
   * If there is a second buyer (like a spouse or business partner), the agent clicks **"+ Add Another Buyer"** and fills in their info.
4. **Choose Representation Model:**
   * Selects **"Designated Representation"** (the agent specifically represents the buyer) or **"Self-Represented Party"** (the buyer is buying on their own without agent representation).
5. **Create Buyer Workspace:** The agent clicks **"Create Transaction"**. The system creates a new deal file and opens the buyer's dashboard.

---

## 3. Real Estate & Legal Rules (Plain English)

* **Legal Names Only:** Nicknames (like "Bob" instead of "Robert") are not allowed because Ontario legal purchase contracts require legal names matching government photo ID.
* **Complete Contact Info:** Every buyer must have a valid email, phone number, and Ontario residential address.
* **In-House Privacy Wall (TRESA Rule):** If Agent A represents the buyer and Agent B in the same brokerage represents the seller, Agent B must be blocked from seeing Agent A's buyer file.

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] Click "+ New Transaction", choose "Buyer", and select "Freehold Resale".
- [ ] Enter a buyer's legal name, phone, email, and Toronto address.
- [ ] Click "+ Add Another Buyer" and add a spouse.
- [ ] Click "Create Transaction" and confirm the deal file opens with both buyers displayed.
- [ ] Check privacy: log into a colleague's account in the same brokerage and verify they cannot see this buyer file.

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary>&lt;b&gt;Click to expand developer specifications (Schemas, APIs &amp; Checklists)&lt;/b&gt;</summary>

### **TypeScript Data Schema**

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
}
```

### **Endpoints to Implement**

* **`POST /api/v1/transactions` — Creates transaction entity in `ONBOARDING` state (Status: `201 Created`).**
* **`POST /api/v1/transactions/{id}/parties` — Adds co-buyers to an existing transaction.**

### **Developer Checklist**

- [ ] **Create PostgreSQL `transactions` and `parties` tables using Prisma.**
- [ ] **Add Row-Level Security (RLS) middleware scoped by `brokerageId` and `agentId`.**
- [ ] **Validate Ontario postal code format with Zod regex (`/^[A-Z]\d[A-Z] \d[A-Z]\d$/i`).**
- [ ] **Emit `TRANSACTION_CREATED` audit event to append-only log.**
- [ ] **Automated tests passing for QA cases `T-001` and `T-004`.**

</details>
