---
title: "M08 — Data, Privacy & Compliance Task Management"
version: "3.0"
date: "2026-10-22"
module: "M08 Task Management"
parent: "[[compliance/index|M08 — Data, Privacy, Legal & Regulatory Compliance]]"
tags:
  - tasks
  - sprints
  - kanban
  - m08
  - compliance
  - tresa
  - fintrac
  - broko
---

# M08 — Data, Privacy & Compliance Task Board

Welcome to the task board for **[[compliance/index|M08 — Data, Privacy, Legal & Regulatory Compliance]]**. This board tracks delivery milestones from TRESA, RECO, FINTRAC (5-year archives), and CASL compliance frameworks to AWS WAF hardening and zero-trust security architecture.

---

## Active Milestone: Sprint 1 — Legal Compliance Framework & Security Hardening

> **Objective:** Execute a dedicated 2-day sprint documenting statutory guardrails (PIPEDA, TRESA, FINTRAC, CASL, VOW), stand up AWS WAF + Cloudflare Pro, and enforce mandatory MFA, AWS Secrets Manager, and encrypted audit logging.  
> **Module Owner:** Br3 Mihail (Admin & Compliance Officer) & Ma1 Praveen (Head of Infrastructure)  
> **Contributing Team:** Br1 Pankaj (Broker of Record), Ma4 Debasis, Ma9 Tantusree  

---

## Master Task Board

| Ticket ID | Task Name | Owner | Sprint | Priority | Status | Dependencies |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`BRK-M08-001`** | **Complete Legal Compliance Framework:** 1-2 day dedicated sprint documenting PIPEDA, TRESA, FINTRAC, CASL, VOW | Br3 Mihail | S1 | 🔴 Critical | Ready | Immediate Priority |
| **`BRK-M08-002`** | **Cybersecurity & Hardening:** AWS WAF + Cloudflare Pro, Zero Trust, MFA enforcement, Secrets Manager, audit logs | Ma1 Praveen | S1 | 🔴 Critical | Ready | M08.1 (Framework documented) |
| **`BRK-M08-003`** | **Proprietary Data Strategy & Governance:** Architecture and compliance protocols for compounding Broko data moat | Br1 Pankaj | S2 | 🟠 High | Backlog | M08.1, M08.2 (Infra live) |

---

## Task Workflow Stages

1. **Backlog:** Future regulatory adjustments, Bill C-27 compliance reviews, and annual audits.
2. **Ready (`ready`):** Groomed statutory checklists, compliance matrices, and security policy drafts.
3. **In Progress (`in-progress`):** Policy drafting, Terraform security rules, or AWS WAF configuration.
4. **Review & Gate:** Legal counsel review, Broker of Record sign-off, and vulnerability penetration testing.
5. **Done:** Enforced across infrastructure and verified in staging/production environments.
