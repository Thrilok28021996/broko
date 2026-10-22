---
title: "Transcript: Counter-Offers, Email Ingestion & Confirmation of Acceptance"
date: "2026-10-12"
type: transcript
status: ingested
attendees:
  - Pankaj
  - Arnab P
  - Thrilok (Speaker 1)
  - Kanchan
tags:
  - transcript
  - meeting
  - retracct
  - counter-offers
  - email-ingestion
  - coa
  - strike-through
---

# Meeting Transcript — 2026-10-12 Counter-Offers, Email Ingestion & Confirmation of Acceptance

> **Source:** Raw Transcript Ingestion (`retracct/meetings/raw/AI Bots Module (1).docx`)  
> **Date:** October 12, 2026 — 13:11 EST  
> **Duration:** 45m 14s  
> **Attendees:** Pankaj, Arnab P, Thrilok (Speaker 1), Kanchan  
> **Traceability:** Requirements [[retracct/05-matrices/requirements-matrix|TM-004, TM-007, TM-010, TM-011]] | Decisions [[retracct/05-matrices/decision-register|Q07, Q09, Q17]] | Tasks [[retracct/tasks/TASK-003-form-100-drafting|TASK-003]], [[retracct/tasks/TASK-005-signed-pdf-import|TASK-005]], [[retracct/tasks/TASK-009-ai-clause-assistant|TASK-009]]

---

## Executive Summary & Key Takeaways

This 45-minute in-depth operational walkthrough by Pankaj clarified critical Ontario real estate practices regarding counteroffers, email-based PDF ingestion (SkySlope-style), document continuity, and final deal acceptance:

1. **Email-Inbound Deal & Counteroffer Ingestion (SkySlope Pattern):**
   - Real estate agents receive offers and counteroffers primarily via email from cooperating agents.
   - Instead of forcing manual download $\rightarrow$ login $\rightarrow$ upload steps, BROKO will provide a dedicated inbound email routing address (e.g. `offers@broko.ai` or dedicated per-agent addresses).
   - An agent simply forwards the email containing the counteroffer PDF to this address.
   - The backend identifies the sender's agent account, extracts the attached PDF, captures the email timestamp, and stages the document in the agent’s transaction workspace, ready for review.

2. **Counteroffer Mechanics in Ontario Practice (Strike-Through & Initialing):**
   - **Document Session Continuity:** RECO best practice is to maintain **the same contract document chain** across counteroffer rounds rather than creating detached, disconnected PDFs.
   - **Round 1 (Seller Counter):**
     - Seller crosses out Buyer's offer price (e.g. $1,150,000 crossed out, $1,245,000 written in) and initials beside the change.
     - Irrevocability is crossed out and flipped from Buyer to Seller with an extended expiry date/time (+1 day).
     - The contract document flips ownership: it is now legally the Seller’s offer to the Buyer.
   - **Round 2 (Buyer Re-Counter):**
     - Buyer crosses out Seller's $1,245,000 (often in red line), writes $1,175,000, initials beside it, flips irrevocability back to Buyer, and signs the Buyer Counter Offer section.
   - **Negotiation Velocity:** 99% of Ontario residential negotiations conclude within 2 to 3 counteroffer rounds.

3. **Confirmation of Acceptance (COA) vs. Acknowledgement:**
   - **Confirmation of Acceptance (COA):** The party who accepts the final, unchanged negotiated terms must sign the COA block on OREA Form 100. This legally locks the contract into an executed agreement.
   - **Acknowledgement Block:** After COA is signed, the other party executes the Acknowledgement confirming receipt of a true signed copy of the accepted agreement.
   - Precise timestamps on COA and Acknowledgement are mandatory for RECO compliance and deposit 24-hour countdown triggers.

4. **AI Counteroffer Diff & Summarization Bot:**
   - When a counteroffer is ingested (via email or upload), the AI assistant analyzes the changes:
     - *"Price changed from $1,150,000 to $1,245,000."*
     - *"Irrevocable deadline extended to Oct 21, 11:59 PM."*
     - *"Deposit and Schedule A conditions unchanged."*
   - Displays the change summary to the agent with 3 immediate action options: **Accept**, **Counter (Round N)**, or **Reject**.

5. **Property Valuation / HPI Bestimates Note:**
   - Discussion regarding property valuation models (OLS standard estimate, advanced bestimate, and HPI adjusted bestimate).
   - Agreed to temporarily suppress the HPI-adjusted figure on production pending QA recalibration, keeping the advanced bestimate active.

---

## Decisions & Resolutions

- [x] **Adopt Inbound Email Ingestion:** Support forwarding signed PDFs to a dedicated BROKO email gateway (`offers@broko.ai` / agent-mapped) as a first-class ingestion channel alongside direct UI uploads.
- [x] **Enforce Single-Session Document Continuity:** Model counteroffers as sequential revisions (`v1`, `v2`, `v3`) of the same underlying offer entity, preserving historical strike-outs, initials, and irrevocability flips.
- [x] **Model Confirmation of Acceptance (COA) Separately from Acknowledgement:** Explicitly track both the COA execution event and the subsequent delivery Acknowledgement timestamp in the transaction state machine.
- [x] **Add AI Counteroffer Difference Summarizer:** Integrate an automated diff summary into the offer register to highlight price changes, date alterations, and modified clauses for the agent.

---

## Action Items & Cross-References

| Action Item | Owner | Target Artifact | Status |
| --- | --- | --- | --- |
| Enhance `TASK-005` to include Email Gateway Ingestion & attachments parser | Backend Eng | [[retracct/tasks/TASK-005-signed-pdf-import]] | Updated |
| Update `TASK-003` offer state machine to model Counteroffer Rounds, COA, and Acknowledgement | Product Lead | [[retracct/tasks/TASK-003-form-100-drafting]] | Updated |
| Add AI Counteroffer Diff feature to `TASK-009` | AI Eng | [[retracct/tasks/TASK-009-ai-clause-assistant]] | Updated |
| Document Inbound Email Webhook in Engineering Specification | Architecture | [[retracct/03-compliance/engineering-compliance]] | Planned |

---

## Verbatim Transcript

<details>
<summary><b>Click to expand raw transcript (AI Bots Module (1) — 2026-10-12)</b></summary>

```text
AI Bots Module-20261012_184157-Meeting Recording
12 October 2026, 01:11pm
45m 14s
Thrilok started transcription

Pankaj   0:03
So what SkySlope does is, let's say I make an offer and I send an offer for signing. It goes to them for signing. That's an e-mail, not my regular e-mail, Pankaj at broker.ai. They have their e-mail through which it goes, server or whatever, right? They sign it, everything's done. Now when I receive a counter offer copy.

Arnab P   0:04
Yeah. Okay.

Pankaj   0:23
Correct. So when I receive a counter offer from some listing agent, I have a dedicated e-mail where I send that counter offer and it gets uploaded into my backend automatically, which can be retrieved when I want.

Arnab P   0:32
Yeah. The. So that agent, how will get that your particular e-mail?

Pankaj   0:44
He sends me on my normal e-mail, Pankaj at broker.ai. Me, I send on that particular e-mail, you know, let's say signing at example, signing at broker.ai. With a, let's say, you know, some, generally we don't put that number of, you know, offer number and what, but we can just put it, you know, simple counter offer for this and this property and it goes, understands and gets stored into our...

Speaker 1   1:12
See.

Pankaj   1:13
Agent wise, let's say example me, I'm an agent right now and I have and we have 100 agents and I have an account at retract. Now my e-mail is connected that all e-mail coming to my, you know, to from Pankaj.broker.ai at broker.ai goes into my back end. So if I send any e-mail on that specific e-mail it gets stored into my database or my forms which I can access. You got it?

Arnab P   1:44
Yeah.

Pankaj   1:44
Okay, let me show you practically little you got it right then I don't need to show it.

Arnab P   1:50
Hmm, I got it, but I'm thinking about how the mail is connected to your back end.

Pankaj   1:50
Yeah, so... Okay, yeah, that solution I don't know. Let's say I'm giving an example.

Arnab P   2:07
Is there a specific tag or something like that that reads it in the menu?

Pankaj   2:12
Let's say this is the offer, right? This is a offer I received, let's say, right? And I want to send it to, so I just what I do is forward and that is my offer. Let's say it says backend at BROKO or whatever I make. Once it goes to the backend at broker.ai, correct? It directly...

Arnab P   2:35
Okay, you send it to the particular mail ID.

Pankaj   2:38
Yeah, let's make that, yeah, that that that way till disaggregated, because if we integrate the whole, if unless you want to integrate the whole e-mail, but we don't need to actually. So, what we can do is we can just keep a specific e-mail. For everyone, for all agents, and what agents can do is they can send it to, let's say, you know, counteroffer, whatever, right? ABC at broker.ai. Through that, it goes into the data. These all files, whatever I want, is stored now in my own portal, you know, and can be retrieved directly on my retract tool so that I don't have to go and check, you know, let's say I want to pick up this one, right? I don't have to go and pick up this one. It can directly come. Otherwise, it's a manual process I download.

Arnab P   3:20
Yeah. Then, in this case, we have to assign a BROKO e-mail address to every real, right? I think.

Pankaj   3:33
Yeah, yeah, even if it is not assigned, doesn't matter, no? We can just put his e-mail address. So that is one thing. Second thing what we will do is... Like, you know, you see this? Like, I'll tell you one thing. Okay, let's say I want to send this listing correct to my portal. One option is I upload this, correct? That's what we decided, PDF file, right? There is another way we can do. In this PDF file, everything comes, contract date, expiry date, whatever is required. That's one option. Second option we can do is just e-mail.

Pankaj   18:24
Then signing comes here, signing is auto-populated, signing is done, send for signing, done. Now second step, let's say we receive a counter offer on the property like this, right? The subject is clear, the intent is clear, many things would be clear here. We forward it to whatever we want at BROKO dot AI. OK. From here, it's again received into the back end. It's not retrieved yet. So it just goes and say, hi. And it says, answer would be, what do you want? Work on counter offer or whatever, right? BROKO bot says, not BROKO bot, whatever, a retract bot, retract says, you work on this. OK, yes, it's retrieved back. We have summarise the changes, this change, this change, this change, this change, this change. You know, it like tells you what has been changed into the counter offer. Summary is there. Okay, let's make a counter offer. Change the price, change this, change this, whatever entry he wants to put now, right? He can put the entries. And again, a PDF appears with the entries, changes done, price change to this, date change to this, whatever changes he said, like the agent says, is done. Once done, send. And that's how this loop continues of offer and counter offer, and everything's getting recorded at, you know, back end.

Arnab P   19:55
Then counter of the then the real offer and the counter offer will be a different session.

Pankaj   20:04
The same offer, so okay, what happened? Like, I will show you one thing. Let's say this is an offer. 700 square feet. OK, and it says no, not twenty-four. Let's do it thirty-six, then crosses it, no rent-free period, right? And the rental also is crossed. So, this offer then will be signed by Landlord, so he will do the same piece of document is my offer right now, but when landlord will change it, he will cross this tenant. He will write now, it's landlord who's making this offer, and then the same piece of document becomes landlord's offer. He can do two things. He can just cross my paper and get a new paper, right? And make it on a new, or he can just cross the existing paper I sent, paperwork I sent, and make changes to it and make this my offer as his offer now. So offer remains same, documentation remains same, but now it becomes his offer, which is called counter offer. Then it comes to us. Then we will do some changes. And then it again becomes a counter offer from us. Counter offer on his offer.

Arnab P   21:40
Then is better to keep one session for it, just we have to sync everything.

Pankaj   21:45
Yes, yes, yes. Yeah, yeah, we should keep one session only because this thing is as per Rico. It's a best practice. As long as possible, keep the same document so that we have time stamping and we have, you know, everything recorded. And this DigiSign verified number is great. But we can also do it, you know, short URL type of things, something like that, so that we can have multiple timestamps over it.

Arnab P   22:37
So how after this, how you will manage? You cannot show any crossover, right? You have to correct it.

Pankaj   22:56
This was the real-time example I'm showing you, OK? This was an offer made by me at 40 Roseville Crescent, Halton Hills. I sent an offer of 1150 50,000 sent by a buyer. These guys didn't accept it, and they sent me a counteroffer on the same e-mail, OK? What this guy did is he filled up his details where he had to. He crossed the price from 1150 and did 1245, crossed the price here, did 1245, and then whatever change is done from whichever side, there is a rule we have to do initial. Crossed, initial 1245. Crossed, now irrevocability changed. Now this offer becomes seller's offer. Change from buyer to seller. Give me more time, one day more. And he changed nothing else.

Pankaj   24:25
After this, I sent another counter offer to them. OK, so this was sign back from my side now. Now I filled for buyer counter offer complete the following. This is me set filled by third time. On the document. He, I sent 1150, he crossed, initialed, then I crossed with red line and initialed, and you know, put my number. I crossed with red line and put my number. With the same session, we will avoid confusion. So this becomes my offer, right? Third offer, which I made. Cross his 1245, sent my 1175, changed here, made it my offer, cross seller, made it buyer. Give him another one day more, 19 to 20 to twenty-first now.

Speaker 1   27:00
How long does it take? Like, this two one for a month?

Pankaj   27:02
Two days, three days, sometime one day, two day, three day. There is no limit, but generally, 99% transactions end within two or three rounds. Nobody goes that long.

Pankaj   28:44
Now, on my final change, when they accepted it, they signed that they accepted. In the end, what she did is, this is the last stage. The last person who is accepting thing signs here confirmation of acceptance that I now accept the final transaction, final deal. So this is how it's complete now. And then they sign acknowledgement. Whoever accepts this signs this first. And then when it comes to me that, okay, these guys have accepted, I tell my client on paper, okay, they have accepted it. Take it now. She accepted on 10th of October. She acknowledged on 10th of October. And I just got it acknowledged 7th. You're getting this?

Pankaj   30:08
Connecting the e-mail will actually be more easier for us in terms of flow and operation, if we can do it, because they can just send it to an e-mail which is attached to their account and everything we can just put it on the back end for them ready to be used with time stamping, e-mail stamping. Storing everything in terms of when the offer was sent, when the offer was received, all those things will make our audit very good.
```

</details>
