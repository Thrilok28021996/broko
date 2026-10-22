---
title: "Transcript: AI Bots Module & Clause Library Discovery"
date: "2026-10-15"
type: transcript
status: ingested
attendees:
  - Pankaj
  - Arnab P
  - Vikas
  - Thrilok (Speaker 1)
tags:
  - transcript
  - meeting
  - retracct
  - ai-bots
  - clauses
  - schedule-a
---

# Meeting Transcript — 2026-10-15 AI Bots Module & Clause Library Discovery

> **Source:** Raw Transcript Ingestion (`retracct/meetings/raw/AI Bots Module.docx`)  
> **Date:** October 15, 2026 — 13:55 EST  
> **Duration:** 6m 43s  
> **Attendees:** Pankaj, Arnab P, Vikas, Thrilok (Speaker 1)  
> **Traceability:** Requirements [[retracct/05-matrices/requirements-matrix|TM-012, TM-013, TM-020]] | Decisions [[retracct/05-matrices/decision-register|Q13, Q14]] | Tasks [[retracct/tasks/TASK-003-form-100-drafting|TASK-003]], [[retracct/tasks/TASK-009-ai-clause-assistant|TASK-009]]

---

## Executive Summary & Key Takeaways

This discovery session clarified how standard OREA clauses, brokerage clause libraries, and the proposed AI Bots / Intelligence Layer operate during offer drafting for Schedule A conditions:

1. **Standard Clause Booklet & Longevity:**
   - Real estate transactions rely on an established clause booklet containing approximately **10 default clauses** that have remained standard in Ontario practice for 30–40 years.
   - Core standard categories include:
     - **Mortgage Conditions:** Arranging a new mortgage, approval to assume existing mortgage, mortgage as a percentage of purchase price, interest rate limits (e.g. "not to exceed 4% per annum"), loan principals, and terms.
     - **Inspection Conditions:** Property inspection by a qualified home inspector within specified business days.
     - **Deposit Conditions:** Balance of purchase price, deposit increase installments.
     - **Title & Legal Conditions:** Lawyer conditional approval / document review.
     - **Property Access Conditions:** Pre-closing access visits for measurements, inspections, and buyer walkthroughs.
     - **Property Insurance Conditions:** Securing satisfactory fire/homeowners insurance.
     - **Assignment Conditions:** Assignment rights and permissions.
     - **Tenancy / Lease Conditions:** Conditional upon lease approval or vacant possession.

2. **Personal Clause vs. Office Clause Libraries:**
   - The system must distinguish between:
     - **Office Clauses:** Brokerage-approved master clause library (managed by BOR/compliance).
     - **Personal Clauses:** Agent-specific customized clauses saved for frequent reuse.

3. **Interactive AI Bot Parameterization Flow:**
   - Rather than forcing agents to copy-paste or write legal clauses from scratch, the **AI Bot / Intelligence Layer** guides the agent interactively:
     - *Agent Action:* Selects "Mortgage Condition".
     - *Bot Prompt:* "Mortgage condition for 5 business days? Anything else you want to mention (e.g., maximum interest rate, principal amount)?"
     - *Bot Output:* Injects the exact legal template with variables populated (e.g., 5 business days, 4.0% per annum, $500,000 principal).

4. **Custom Clause Generation with Human-in-the-Loop:**
   - Agents can supply natural language intent (e.g., "buyer wants to re-inspect pool pump before closing").
   - AI bot synthesizes the clause based on standard clause booklet conventions.
   - **Compliance Guardrail (Q14, TM-020):** AI-generated custom clauses must require explicit agent review and cannot be silently inserted into an executed contract.

---

## Decisions & Resolutions

- [x] **Baseline Standard Clause Library:** Seed the system with the 10 standard OREA clause templates across mortgage, inspection, lawyer approval, access, and insurance.
- [x] **Dual Library Scoping:** Support both `Office` (brokerage-wide) and `Personal` (agent-level) clause libraries.
- [x] **Parameterized Variable Engine:** Structure all clauses with typed variables (`days`, `rate_cap`, `principal_amount`, `inspection_scope`) to power both manual fill and bot conversational prompts.
- [x] **Strict Human Review Gate:** In accordance with PRD Decision `Q14`, AI drafts are proposals only and require human agent confirmation before appending to Schedule A.

---

## Action Items & Cross-References

| Action Item | Owner | Target Artifact | Status |
| --- | --- | --- | --- |
| Create dedicated task `TASK-009` for AI Clause Assistant & Parameterization Engine | Product / Tech Lead | [[retracct/tasks/TASK-009-ai-clause-assistant]] | Planned |
| Enhance `TASK-003` (Form 100 Drafting) with Schedule A clause insertion & variable schema | Tech Lead | [[retracct/tasks/TASK-003-form-100-drafting]] | Done |
| Define Clause Definition schema with personal/office tags and variable placeholders | Engineering | [[retracct/03-compliance/engineering-compliance]] | In Progress |
| Cross-reference PRD clause requirements with Decision Register Q13/Q14 | Compliance | [[retracct/05-matrices/decision-register]] | Done |

---

## Open Ambiguities / Legal Flags

- **Source Clause Booklet Licensing:** Ensure clauses loaded into the default system are verified against current OREA standard wording without infringing copyright or using outdated phrasing.
- **Reference:** [[retracct/05-matrices/open-decisions|Open Decisions A-01 & A-02]]

---

## Verbatim Transcript

<details>
<summary><b>Click to expand raw transcript (AI Bots Module — 2026-10-15)</b></summary>

```text
AI Bots Module-20261015_192458-Meeting Recording
15 October 2026, 01:55pm
6m 43s
Thrilok started transcription

Arnab P   0:05
on me.

Pankaj   0:09
There is a clause booklet. And then we have to give an option to make it, let me try to find out my ID password. I exactly don't remember.

Speaker 1   0:18
Right. Is there like a default clauses or on other classes will change like a year or year?

Pankaj   0:33
10 default closes. They have been same for almost 30, 40 years now.

Speaker 1   0:37
Ohh.

Pankaj   0:38
Yeah, same clothes. Password, password.

Speaker 1   1:12
Eight, 10. Yeah.

Pankaj   1:49
Where do I send you? How do I, okay, I have a question now. How do I add this thing to here, like upload PDF here?

Speaker 1   1:58
Like. Please. Shh. Yeah, you are, yeah, correct only.

Pankaj   2:11
I don't have the option. It doesn't let me allow any uploading.

Speaker 1   2:13
Yeah, OK, OK, sharing, sharing, sharing, I think the reason is a BROKO dot AI, right? All ours are in Mantra and you are in BROKO dot AI, so that's the problem.

Pankaj   2:26
No, even I think.

Speaker 1   2:28
No, you so what it's saying that it's saying that you are in different organisation, so you don't have the access. So, that's what I had, I think transaction management, thanks.

Pankaj   2:37
Where is, where did I?

Arnab P   2:39
It's not showing for me also.

Pankaj   2:44
It doesn't let me do anything. Look, even I've tried earlier also. I think our team has some restrictions, so I will talk to Haresh about it. So these are the clauses, conditions, right? Accessing condition, assign condition, then there are mortgage conditions. So offer is always sent with that condition. So deposit balance of purchase price, deposit increase. So there are some default conditions we use and there are some custom conditions which we can. Put as per our requirement.

Speaker 1   3:13
K.

Pankaj   3:15
insurance, lease, lease approval, lawyer conditional approval, table of, and then there is mortgage condition, mod 1 approval to assume existing mortgage, arrange in a new mortgage, arrange a new mortgage as percentage of these all. So explanation is given and mortgage is given. So there are just these variables which will be needed eventually. Like a new mod dash less than dash. So this is all intelligence layer the look.

Speaker 1   3:42
No, no, okay, okay, okay, this is okay, but this is the form that we are going to submit, or I mean, that is going to do to the user, or...

Pankaj   3:52
No, so how we work is... I'll show you. Wait.

Speaker 1   4:01
Because we have like a form 300 kind of forms like that.

Pankaj   4:04
No, and I'll show you what how it works is. Let's say I made this offer, let's say... OK. I have to go on a call in a minute. So I'll just keep, so I'll just show you quickly. For example, I go here and I add a clause. Which clause I want to, personal clause or office clause? Okay, I want to add, let's say, mortgage clause.

Speaker 1   4:23
Okay. Sure.

Pankaj   4:36
Right, so it's gonna come like me. OK, arranging, arranging a new mortgage. Let's add that. OK, you wanna see this clause? This is a clause. OK, let's. Okay.

Speaker 1   4:49
Okay, okay. Only content from the PDF file, right?

Pankaj   4:51
Right. It just gets downloaded and this whole thing has to be filled as well. So we will ask the buyer, you know, the user, what do you want to put? You know, like I put this 4% annum, you know, for $5,000. These are some variables which we will ask.

Speaker 1   5:02
Deepak.

Arnab P   5:02
It. You.

Speaker 1   5:09
Okay, okay.

Pankaj   5:13
Like if we say mortgage condition, then the bot will ask, okay, mortgage condition for five business days. Anything else you want to mention? And accordingly, it picks up the clauses for five business days or four business days. That's how it'll be.

Speaker 1   5:27
Okay, okay.

Pankaj   5:29
Okay, I think I have to go on a call, so we'll connect. Hannah.

Arnab P   5:31
Okay.

Speaker 1   5:32
Okay. Shirsale.

Arnab P   5:35
Okay.

Pankaj   5:35
You got it right. Like you got an idea right how the clauses will be added and there will be custom clauses also an option to make custom clauses also.

Arnab P   5:37
Mhm.

Speaker 1   5:38
Yes, yes. Okay.

Pankaj   5:44
Like, you know, simple ChatGPT, go in ChatGPT, right? I want to make a mortgage clause. You know, upload this file Thrilok on ChatGPT workflow, say, make it easy to use, custom able, accordingly, understand the intent example, and then give me the easier version of it, which we can use it in our. Total.

Speaker 1   6:05
Okay.

Pankaj   6:06
Right, OK, then. Thank you, bye.

Speaker 1   6:09
And. Arnab, you got an understanding, right?

Arnab P   6:17
Yeah, okay.

Speaker 1   6:18
Or else I will, I also I'll let you know, I mean, or else I will explain later, if, if not, if we have anything else.

Arnab P   6:27
Sure.

Speaker 1   6:28
Okay. OK, then, uh, because anything else you want to discuss?

Vikas   6:30
Ohh. The. No, I am good.

Speaker 1   6:36
OK, OK, then, time, time to Arnab, time to Vikas, yeah, take care.

Arnab P   6:39
Bye, bye.

Thrilok stopped transcription
```

</details>
