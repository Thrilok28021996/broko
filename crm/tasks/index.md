---
title: "M02 — Lead Generation & CRM Task Management"
version: "3.0"
date: "2026-10-22"
module: "M02 Task Management"
parent: "[[crm/index|M02 — Lead Generation & CRM]]"
tags:
  - tasks
  - sprints
  - kanban
  - m02
  - crm
  - broko
---

# M02 — Lead Generation & CRM Task Board

Welcome to the task board for **[[crm/index|M02 — Lead Generation & CRM]]**. This board tracks delivery milestones from multi-channel lead ingestion <60s and XGBoost lead scoring to automated nurturing and RBAC dashboards.

---

## Active Milestone: Sprint 1 — Ingestion Pipeline, Scoring Engine & RBAC

> **Objective:** Stand up the sub-60s multi-channel capture pipeline, XGBoost 0–100 lead scoring engine, automated territory assignment, and role-based CRM access.  
> **Module Owner:** Ma4 Debasis (Technical Lead)  
> **Contributing Team:** Ma5 Arnab (Backend/MongoDB), Ma3 Thrilok (AI/ML Scoring), Br4 Ivan (Sales Manager), Br5 Deepanshu  

---

## Master Task Board

| Ticket ID | Task Name | Owner | Sprint | Priority | Status | Dependencies |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`BRK-M02-001`** | **Multi-Channel Lead Auto-Flow:** Website forms, Meta Ads API, Google Ads API feeding CRM <60s with deduplication | Ma4 Debasis | S1 | 🔴 Critical | Ready | M04 (Website live) |
| **`BRK-M02-002`** | **AI Lead Scoring Engine:** Score 0–100, classify Hot/Warm/Cold, portal vs unverified tiers via XGBoost & Redis | Ma3 Thrilok | S1 | 🔴 Critical | Ready | M02.1 (Lead capture live) |
| **`BRK-M02-003`** | **Intelligent Lead Distribution:** Auto-routing by territory, property type, price, agent availability with SLA timer | Ma4 Debasis | S1 | 🔴 Critical | Ready | M02.2 (Lead scoring live) |
| **`BRK-M02-004`** | **Behaviour-Triggered AI Nurturing:** Automated WhatsApp/SMS/Email sequences (Wati.io, Twilio) + CASL audit log | Ma5 Arnab | S2 | 🟠 High | Backlog | M02.3 (Lead distribution live) |
| **`BRK-M02-005`** | **RBAC CRM Dashboards:** Role-specific portals for Admin, Team Lead, Realtor, Accounting, Reception | Ma4 Debasis | S1 | 🔴 Critical | Ready | M02.1–M02.3 (Core pipeline live) |
| **`BRK-M02-006`** | **AI Voice Bot for Hot Leads:** 60-second outbound call via Vapi.ai + ElevenLabs to qualify and book agent callback | Ma3 Thrilok | S3 | 🟠 High | Backlog | M02.2 (Scoring live), M07 (Bot system) |

---

## Task Workflow Stages

1. **Backlog:** Future enhancements and integrations scheduled for upcoming sprints.
2. **Ready (`ready`):** Groomed user stories with defined payload schemas and endpoints ready for development.
3. **In Progress (`in-progress`):** Implementation in Node.js/MongoDB or Python/XGBoost.
4. **Review & Gate:** End-to-end webhook validation, deduplication stress testing, and CASL compliance audit.
5. **Done:** Deployed to AWS environment and integrated with live ingestion pipelines.
