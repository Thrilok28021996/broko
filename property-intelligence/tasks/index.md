---
title: "M05 — CMA & Property Intelligence Task Management"
version: "3.0"
date: "2026-10-22"
module: "M05 Task Management"
parent: "[[property-intelligence/index|M05 — CMA & Property Intelligence]]"
tags:
  - tasks
  - sprints
  - kanban
  - m05
  - cma
  - property-intelligence
  - broko
---

# M05 — CMA & Property Intelligence Task Board

Welcome to the task board for **[[property-intelligence/index|M05 — CMA & Property Intelligence]]**. This board tracks delivery milestones from the 3-pillar valuation model audit (99% accuracy target) and 10-second branded CMA report generator to Property Snap camera integration and MLflow Sunday retraining pipelines.

---

## Active Milestone: Sprint 1 — Model Accuracy Audit, 10s Report Generator & VPC Boundary

> **Objective:** Execute full 12-month sold data MAPE audit across all property types, complete AWS Lambda ReportLab PDF generator in <10 seconds, and enforce strict VPC perimeter isolating MLS weights from Claude API.  
> **Module Owner:** Ma3 Thrilok (AI/ML Engineer — **Primary Technical Executor**)  
> **Contributing Team:** Ma1 Praveen (Infra/VPC Boundary), Ma4 Debasis (Alerts), Ma12 Sujit (Report Design), Br1 Pankaj (CEO & BOR)  

---

## Master Task Board

| Ticket ID | Task Name | Owner | Sprint | Priority | Status | Dependencies |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`BRK-M05-001`** | **CMA Model Audit & Accuracy Sprint:** Audit 3 model pillars vs 12mo sold data, calculate MAPE by property type | Ma3 Thrilok | S1 | 🔴 Critical | Ready | Immediate Week 1 Priority |
| **`BRK-M05-002`** | **Branded PDF & Digital Report Generator:** 10-second generation via AWS Lambda & ReportLab | Ma3 Thrilok | S1 | 🔴 Critical | Ready | M05.1 (Accuracy acceptable) |
| **`BRK-M05-003`** | **Strict Data Boundary Enforcement:** VOW/MLS sold data stays in AWS VPC; Claude receives non-sensitive summaries | Ma1 Praveen | S1 | 🔴 Critical | Ready | Non-negotiable Week 1 Gate |
| **`BRK-M05-004`** | **Real-Time Property & Price Alerts:** Notifications within 5m of MLS change; load tested for 1,000 concurrent | Ma4 Debasis | S1 | 🔴 Critical | Ready | M05.1 (CMA), M02.1 (CRM) |
| **`BRK-M05-005`** | **Agent Rating System & Sunday Retraining:** Thumbs up/down UI in portal triggering MLflow Sunday pipeline | Ma3 Thrilok | S2 | 🔴 Critical | Backlog | M05.2 (CMA reports live) |
| **`BRK-M05-006`** | **AI Renovation Estimator & Neighbourhood Stats:** GTA cost calculator + HoodQ/WalkScore school & transit metrics | Ma3 Thrilok | S2 | 🟠 High | Backlog | M05.2 (CMA reports live) |

---

## Task Workflow Stages

1. **Backlog:** Future ML model iterations, Computer Vision enhancements, and renovation estimator expansions.
2. **Ready (`ready`):** Groomed ML specifications with defined feature vectors, data schemas, and training sets.
3. **In Progress (`in-progress`):** Model training on AWS SageMaker, Lambda authoring, or prompt tuning.
4. **Review & Gate:** Accuracy validation (MAPE <2%), latency benchmarking (<10s generation), and data boundary audit.
5. **Done:** Model weights deployed to production SageMaker endpoint and hooked to user-facing APIs.
