---
name: broko-multi-bot
description: Use when working on the BROKO Multi-Bot AI System, 10 specialized bots, Broko Brain RAG (Pinecone), AI Validation Bot confidence scoring, or LangGraph multi-agent orchestration.
---

# BROKO Multi-Bot AI System — Deterministic Domain Skill

This skill governs all development, prompt engineering, and multi-agent orchestration across the **10 Specialized Bots & Broko Brain (@multi-bot / M07)**.

---

## 1. Non-Negotiable Operational Boundaries

1. **Shared Intelligence via Broko Brain:**
   - All 10 bots must query Broko Brain (central Pinecone vector store via AWS PrivateLink) before synthesizing answers.
   - Every answer must cite its exact source (TRESA section, OREA form clause, or brokerage SOP).
2. **AI Validation Bot Gate (`Loop 8`):**
   - No AI-generated output is delivered to agents or clients without passing through the AI Validation Bot.
   - Outputs receiving a confidence score <70 must be staged for human supervisor review.
3. **No Unassisted Legal Advice:**
   - The BOR Bot and Compliance Bot provide statutory references and advisory checks, but explicitly state they do not replace human licensee judgment.
4. **LangGraph Multi-Agent Protocol:**
   - When conversations transition between roles (e.g. buyer inquiry to offer drafting), context must be preserved via LangGraph stateful memory without exceeding context boundaries.

---

## 2. The 10 Canonical Bots

1. **BOR Bot:** Pankaj WhatsApp co-pilot for RECO/TRESA compliance questions.
2. **Deal Admin Bot:** Mihail co-pilot for transaction checklists & FINTRAC verification.
3. **Agent/Salesperson Bot:** 24/7 objection handling, pricing scripts & SOPs.
4. **Compliance Check Bot:** Automated PASS/FAIL check on draft OREA offers before submission.
5. **Marketing Material Bot:** Auto-generates flyers, captions, and blasts from MLS listings.
6. **Customer Engagement Bot:** 24/7 lead qualification widget on `broko.ai`.
7. **Market Intelligence Bot:** Daily 8:00 AM market digest sent to active agents.
8. **AI Validation Bot:** Mid-tier validator scoring every AI output 0–100.
9. **Agent Hiring Bot:** Ivan's 1-to-many personalized recruitment outreach co-pilot.
10. **Innovation Board Bot:** Monday 8:00 AM automated competitor & strategic brief for CEO.

---

## 3. Key Knowledge Assets

* **Module Index:** [`multi-bot/index.md`](file:///Users/thrilok/Documents/OpenKnowledge/Broko/multi-bot/index.md)
* **Tasks Backlog:** `BRK-M07-001` to `BRK-M07-005`
* **Telemetry & Cost Tracking:** Self-hosted Langfuse on AWS.
