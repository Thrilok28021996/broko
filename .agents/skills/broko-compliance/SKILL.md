---
name: broko-compliance
description: Use when working on BROKO legal compliance, regulatory frameworks (TRESA, RECO, FINTRAC, CASL, PIPEDA, PropTx VOW), cybersecurity controls, or data retention rules.
---

# BROKO Compliance & Regulatory Governance — Deterministic Domain Skill

This skill enforces strict adherence to Canadian federal and Ontario provincial legal frameworks across the entire **BROKO Platform (@compliance / M08)**.

---

## 1. Non-Negotiable Operational Boundaries

1. **CASL Zero-Tolerance Rule:**
   - Every commercial electronic message (Email, SMS, WhatsApp) requires prior express consent with an explicit timestamp and IP address on record.
   - Every message must carry a functioning one-click unsubscribe mechanism.
2. **FINTRAC 5-Year Mandate:**
   - Two valid government identity documents must be verified and cryptographically stored per transaction before binding commitments are executed.
   - Records must be retained for at least 5 years and producible within 48 hours for an audit.
3. **TRESA Representation Requirements:**
   - No offer presentation or trade negotiations may occur without an executed Buyer Representation Agreement (Form 300) or Self-Represented Party Confirmation (Form 320).
4. **PropTx / CREA Data Safeguards:**
   - Sold MLS transaction data must sit behind a mandatory Virtual Office Website (VOW) Terms of Use and registration gate.
   - Raw data must never be shared with unapproved third-party APIs.
5. **AODA Accessibility:**
   - All consumer interfaces on `broko.ai` must conform to WCAG 2.1 Level AA.

---

## 2. Key Knowledge Assets

* **Module Index:** [`compliance/index.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/compliance/index.md)
* **Tasks Backlog:** `BRK-M08-001` to `BRK-M08-003`
* **Audit Protocols:** Snyk vulnerability scanning, CloudTrail immutable logging.
