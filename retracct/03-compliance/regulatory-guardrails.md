---
title: "Source-Backed Regulatory Guardrails"
version: "3.0"
date: "2026-10-20"
parent: "[[retracct/03-compliance/engineering-compliance|Engineering & Compliance]]"
tags:
  - regulatory
  - tresa
  - reco
  - fintrac
  - orea
---

# Source-Backed Regulatory Guardrails

BROKO embeds strict adherence to statutory authorities in Ontario real estate.

---

## 1. RECO / TRESA (Trust in Real Estate Services Act)
- **Source:** [[retracct/05-matrices/regulatory-sources|REG-01]] (RECO TRESA Explained) & [[retracct/05-matrices/regulatory-sources|REG-02]] (O. Reg. 579/05).
- **Multiple Representation:** Governed strictly by brokerage vs designated representation agreements. The system enforces party-isolated permissions and screening firewalls whenever both sides of a trade share a brokerage.
- **RECO Information Guide:** Must be provided and explained to prospective clients before any regulated trading services or assistance are provided.
- **Offer Disclosures:** Mandatory disclosure of competing written offers (number of competing offers, representation status, and written direction).

## 2. FINTRAC (Proceeds of Crime & Anti-Money Laundering)
- **Source:** [[retracct/05-matrices/regulatory-sources|REG-04]] (Recordkeeping) & [[retracct/05-matrices/regulatory-sources|REG-05]] (Client Identification).
- **Client Identity Verification:** Mandatory verification of individual identity (government photo ID or dual-process method) before or at the time of agreement.
- **Receipt of Funds Record:** Mandatory recording of receipt of funds within required timeframes; tracking origin of deposit funds.

## 3. Ontario Residential Tenancies Act (RTA)
- **Source:** [[retracct/05-matrices/regulatory-sources|REG-03]] (Ontario Standard Form of Lease).
- **Mandatory Form:** All private residential leases must utilize the official Ontario Standard Lease. OREA Form 400 is treated only as a preliminary offer to lease.

## 4. OREA Standard Forms Governance
- **Source:** [[retracct/05-matrices/regulatory-sources|REG-06]] (OREA Forms).
- **Form Version Governance:** The system evaluates form version identifiers against the licensed repository.
- **Warn-Only Status:** If the latest version cannot be affirmatively confirmed, the system surfaces a visible advisory banner and alerts the Broker of Record (BOR) queue without halting agent drafting.

---
*See [[retracct/05-matrices/form-and-field-registry|Form & Field Registry]] for exact field validation rules.*
