---
title: "M09 — Platform Infrastructure & DevOps Task Management"
version: "3.0"
date: "2026-10-22"
module: "M09 Task Management"
parent: "[[infrastructure/index|M09 — Platform Infrastructure & DevOps]]"
tags:
  - tasks
  - sprints
  - kanban
  - m09
  - infrastructure
  - devops
  - aws
  - broko
---

# M09 — Platform Infrastructure & DevOps Task Board

Welcome to the task board for **[[infrastructure/index|M09 — Platform Infrastructure & DevOps]]**. This board tracks delivery milestones from production SLA monitoring (99.9% uptime, API <200ms p95) and automated CI/CD (<10m) to systematic cloud cost optimization (<$3,500/mo).

---

## Active Milestone: Sprint 1 — Production Performance, Monitoring & CI/CD Setup

> **Objective:** Deploy AWS CloudWatch dashboard, set up multi-AZ database clustering, configure GitHub Actions CI/CD with automated test gating, and benchmark p95 API response times.  
> **Module Owner:** Ma1 Praveen (Head of Infrastructure)  
> **Contributing Team:** Ma2 Vikas (CTO — Architecture), Ma4 Debasis (API Specs), Ma9 Tantusree (Test Gates)  

---

## Master Task Board

| Ticket ID | Task Name | Owner | Sprint | Priority | Status | Dependencies |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`BRK-M09-001`** | **Production Performance & Monitoring:** CloudWatch dashboard, 99.9% uptime, LCP <2.5s, API <200ms p95, multi-AZ | Ma1 Praveen | S1 | 🔴 Critical | Ready | None (Week 1 Setup) |
| **`BRK-M09-002`** | **Automated CI/CD & API-First Refactor:** Commit-to-prod in <10m, test gates, multi-tenant API spec | Ma4 Debasis | S2 | 🟠 High | Backlog | M10 (QA test suite built) |
| **`BRK-M09-003`** | **Systematic Cost Optimisation:** Claude prompt caching, EC2 Reserved Instances, Spot training, self-hosted tools | Ma1 Praveen | S2 | 🟠 High | Backlog | M09.1 (Monitoring live) |

---

## Task Workflow Stages

1. **Backlog:** Multi-region failover, disaster recovery drills, and multi-tenant billing isolation.
2. **Ready (`ready`):** Groomed infrastructure tickets with Terraform plans and architecture diagrams.
3. **In Progress (`in-progress`):** CloudFormation/Terraform provisioning, CI/CD pipeline authoring, or monitoring configuration.
4. **Review & Gate:** Security review, load testing verification (k6), and failover testing.
5. **Done:** Applied to production AWS environment with automated alerting active.
