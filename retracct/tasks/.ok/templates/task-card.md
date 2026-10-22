---
template:
  title: BROKO Task Card
  description: Template for creating clear tasks understandable by domain experts and developers alike.
  tags:
    - task
    - retracct
title: "Task: [Title]"
date: "{{date}}"
type: task
status: todo
priority: P0
sprint: sprint-1-poc
assignee: "{{user}}"
requirement: "[[retracct/05-matrices/requirements-matrix]]"
qa_case: "[[retracct/05-matrices/qa-cases]]"
tags:
  - task
  - retracct
---

# Task: [Task Title]

> **Status:** `todo` | **Priority:** `P0` | **Sprint:** `sprint-1-poc`  
> **Traceability:** Requirement [[retracct/05-matrices/requirements-matrix]] | QA Case [[retracct/05-matrices/qa-cases]]

---

## 1. What This Feature Does (In Plain English)

[Explain in simple, everyday language what this feature is, what real-world problem it solves, and why it is important for the brokerage or client.]

---

## 2. What the User Sees & Does on Screen (Step-by-Step)

1. **Step 1:** The user clicks...
2. **Step 2:** The screen asks for...
3. **Step 3:** The user enters...
4. **Step 4:** The system displays...

---

## 3. Real Estate & Legal Rules (Plain English)

* **Rule 1:** [E.g., What RECO / TRESA law requires]
* **Rule 2:** [E.g., Privacy or math rule]
* **Rule 3:** [E.g., What is forbidden or gated]

---

## 4. How Anyone Can Test It (Without Technical Knowledge)

- [ ] [Simple action 1 to test in the UI]
- [ ] [Simple action 2 to check]
- [ ] [What you expect to see on screen]

---

## 5. Technical Blueprint for Developers (Code, APIs & Database)

<details>
<summary><b>Click to expand developer specifications (Schemas, APIs & Checklists)</b></summary>

### TypeScript Data Schema
```typescript
// Developer data contract
```

### Endpoints to Implement
* `POST /api/v1/...` — Describe route and HTTP status.

### Developer Checklist
- [ ] Database table migration.
- [ ] Business logic service implementation.
- [ ] Unit & integration tests passing.

</details>
