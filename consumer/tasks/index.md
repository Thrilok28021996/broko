---
title: "M04 — Consumer Platform (broko.ai) Task Management"
version: "3.0"
date: "2026-10-22"
module: "M04 Task Management"
parent: "[[consumer/index|M04 — Consumer Platform — broko.ai]]"
tags:
  - tasks
  - sprints
  - kanban
  - m04
  - consumer
  - broko-ai
  - broko
---

# M04 — Consumer Platform (broko.ai) Task Board

Welcome to the task board for **[[consumer/index|M04 — Consumer Platform — broko.ai]]**. This board tracks delivery milestones from homepage CRO, Bestimate hero CTA, and natural language search to VOW compliance gates and AODA accessibility.

---

## Active Milestone: Sprint 1 — Homepage CRO, VOW Compliance & Free-Flow Search

> **Objective:** Redesign homepage hero with Bestimate as the primary above-the-fold CTA, purge legacy Re/Max branding, enforce mandatory VOW login/Terms of Use before displaying sold data, and deploy natural language search across 100+ Ontario test queries.  
> **Module Owner:** Ma7 Subhashis (Frontend Lead)  
> **Contributing Team:** Ma4 Debasis, Ma6 Souradip, Ma8 Souvik, Ma9 Tantusree, Ma3 Thrilok  

---

## Master Task Board

| Ticket ID | Task Name | Owner | Sprint | Priority | Status | Dependencies |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **`BRK-M04-001`** | **Homepage CRO & Hero Redesign:** Bestimate as primary CTA, above-fold capture, WebP compression, mobile nav fix | Ma7 Subhashis | S1 | 🔴 Critical | Ready | None (Immediate Week 1) |
| **`BRK-M04-002`** | **VOW Compliance & Footer Branding Fix:** Gate sold data behind login/ToU, purge Re/Max branding, FINTRAC notices | Ma4 Debasis | S1 | 🔴 Critical | Ready | None (Legal Risk Today) |
| **`BRK-M04-003`** | **Natural Language Free-Flow Search:** Complete QA with 100+ Ontario test queries, AWS MLS parsing, production rollout | Ma7 Subhashis | S1 | 🔴 Critical | Ready | M04.1 (Homepage stable) |
| **`BRK-M04-004`** | **500+ Programmatic Neighbourhood Pages:** Next.js SSG, local stats, school scores, auto-updating MLS data | Ma4 Debasis | S2 | 🟠 High | Backlog | M04.1 (Stable), M04.3 (Search) |
| **`BRK-M04-005`** | **Floating 24/7 AI Chat Widget:** Lead qualification bot, app download badges, instant valuation teaser | Ma4 Debasis | S2 | 🟠 High | Backlog | M07 (Customer Bot live) |
| **`BRK-M04-006`** | **Full AODA / WCAG 2.1 AA Compliance:** Screen reader testing, keyboard navigation, contrast audit via Axe in CI/CD | Ma9 Tantusree | S2 | 🔴 Critical | Backlog | M04.1 (Fixes complete) |

---

## Task Workflow Stages

1. **Backlog:** Programmatic SEO expansion, native iOS/Android shell features, and advanced chat widgets.
2. **Ready (`ready`):** Groomed frontend PRDs and UI wireframes with responsive breakpoints.
3. **In Progress (`in-progress`):** Next.js component authoring, Tailwind styling, and API integration.
4. **Review & Gate:** Lighthouse performance audit (LCP <2.5s), Axe accessibility gate, and legal VOW sign-off.
5. **Done:** Deployed to Vercel production edge network.
