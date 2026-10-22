---
name: broko-property-intelligence
description: Use when working on the BROKO CMA & Property Intelligence module, valuation models (Hard Value, Soft Features, Market Analysis), Property Snap feature, Bestimate, MLflow Sunday retraining, or AWS data boundaries.
---

# BROKO Property Intelligence — Deterministic Domain Skill

This skill enforces deterministic, source-backed behavior across all AI agents and engineers working on **BROKO (@property-intelligence / M05)**.

---

## 1. Non-Negotiable Operational Boundaries

1. **Strict AWS VPC Data Residency:**
   - **Never export** raw MLS/VOW listing data, individual sold records, client details, or ML model weights outside the AWS VPC.
   - Claude API is strictly whitelisted for non-sensitive reasoning (anonymized market narratives and high-level summaries).
2. **Three-Pillar Valuation Integrity:**
   - Every formal valuation must compute contributions across all 3 pillars:
     * **Pillar 1 (Hard Value):** Physical specs (sqft, bedrooms, bathrooms, lot, garage).
     * **Pillar 2 (Soft Features):** Qualitative condition and finish adjustments.
     * **Pillar 3 (Market Analysis):** Micro-market absorption rate, days on market (DOM), and comparable transactions.
3. **Accuracy Target & Self-Learning:**
   - Target: MAPE <2% (99% accuracy) by Month 6.
   - Automated Sunday night retraining pipeline runs via MLflow only after 50+ agent ratings are accumulated.
4. **Property Snap & VOW Gates:**
   - Point-and-shoot camera capture with confidence scoring (High/Medium/Low).
   - Basic identification is open; sold comparables and final valuation require phone OTP verification per VOW regulations.
   - Estimates must always include a non-appraisal disclaimer.

---

## 2. Key Knowledge Assets

* **Module Index:** [`property-intelligence/index.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/property-intelligence/index.md)
* **Property Snap PRD:** [`property-intelligence/snap-feature-prd.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/property-intelligence/snap-feature-prd.md)
* **Data Boundary Policy:** [`property-intelligence/index.md#5-non-negotiable-data-boundary`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/property-intelligence/index.md#5-non-negotiable-data-boundary)
* **Weekly Automation Schedule:** Every Sunday at 11:00 PM (MLflow retraining).

---

## 3. Query Resolution Protocol

When answering technical or product queries regarding Property Intelligence:
1. **Verify Data Classification:** If the request involves MLS data, enforce the AWS VPC boundary.
2. **Model Pillar Attribution:** Specify which of the 3 valuation pillars handles the attribute.
3. **Traceability:** Cite ticket IDs (`BRK-M05-001` to `BRK-M05-006`) and the specific section in `property-intelligence/index.md`.
