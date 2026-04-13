# Adoption, Feedback, and Quantification of the Agentic Architecture

A structured guide for driving framework adoption in the organization, collecting useful feedback beyond personal experience, and quantifying its impact in a practical way.

---

## 1. How to Drive Adoption

Goal: get more people using the architecture in real workflows, not just attending training sessions.

### 1.1 Scoped Pilot

- Choose **one team or 2–3 people** who commit to using the framework in a real flow (one sprint, one epic).
- Condition: brief feedback at the end (what was hard, what helped).
- Produces concrete cases and evidence for the rest of the organization.

### 1.2 Reduce First-Use Friction

- Define a **very clear first step**: e.g., "for your next ticket: 1) open a chat with @planning, 2) paste the ticket, 3) when you have the plan, use /blueprint".
- Provide a **quickstart** or **pre-filled example** (plan, enrichment, issue) that can be copied.
- Prioritize "concrete next step" over more theory.

### 1.3 Show Results with Data

- Even lightly: "in the pilot we closed N tickets, M PRs, with Z review iterations".
- Or: "before it took N days to have a plan ready, now 1 session".
- Helps sell adoption and lets leadership see improvement, not just "another course".

### 1.4 Integrate into Existing Rituals

- In **refinement** or **planning**: include "plan in .plan.md" or "enrich with /enrich" in the DoR.
- In **retro**: add a question like "did you use the architecture (modes/commands) this iteration?".
- This way the architecture becomes part of the official flow, not something optional and isolated.

### 1.5 Clear Point of Contact

- Define where to resolve questions or blockers: a person (e.g., you) or a specific channel.
- Goal: make it obvious who to ask and ensure answers arrive quickly.
- If people don't know where to start or get stuck without a response, adoption stalls.

---

## 2. How to Collect Feedback (Beyond Your Own Experience)

Goal: ensure architecture improvements are based on real signal from people who use or try it.

### 2.1 Structured Feedback After the Pilot (or After Each Hands-On Session)

- Short format: **3–5 questions**, for example:
  - What was hardest for you?
  - What would you remove or simplify?
  - What did you miss?
  - On a scale of 1 to 5, would you use it on your next ticket?
- Tool: a short form or shared doc; nothing complex needed.
- Important: **close the loop** — communicate "based on your feedback we changed X" in the next session or on an internal channel.

### 2.2 Office Hours or "Architecture Clinic"

- **A fixed hour** weekly or biweekly where anyone using (or trying) the architecture can bring a real case.
- Examples: "I don't know how to fit this ticket", "my plan got too long", "I don't understand when to use @dev vs @planning".
- You solve it live and **note patterns**: what repeats, what confuses, what is missing in documentation or modes.
- Highly valuable feedback without depending on people writing reports.

### 2.3 Observe Real Usage

- Once there is adoption (pilot or otherwise): agree to **review together 1–2 plans and 1–2 flows** (plan → dev → PR).
- No automation needed: seeing how people use it and where they get stuck already provides input for improving contracts, modes, and commands.

### 2.4 Changelog: "What Changed Because of Your Feedback"

- A doc or section (e.g., in the proposal) like: "v3.1: we simplified X because in the pilot we saw that…".
- People see their feedback has impact; you maintain a history of why the architecture evolves.
- Reinforces adoption and forces translating feedback into concrete changes.

---

## 3. Quantification Framework (Six Pillars)

A framework for measuring whether the architecture delivers value. Metrics can be collected manually or lightly (e.g., once per sprint or at pilot close). Common scale: **1–10** per pillar (guiding reference).

### 3.1 Useful Throughput (Value Delivery)

**What it measures:** how much "real" work ships per unit of time.

**What to collect:**
- PRs merged per week that meet DoD.
- Tickets closed per week (optional: weight by size — story points or T-shirt).
- % of PRs reverted or generating hotfixes (to avoid inflating with low-quality deliveries).

**Scale 1–10 (guide):**
- 1–3: sporadic deliveries, no rhythm.
- 4–6: stable rhythm but with bottlenecks.
- 7–10: consistent and predictable cadence.

---

### 3.2 End-to-End Lead Time (Idea → Production)

**What it measures:** efficiency of the complete flow, not just "coding fast".

**What to collect:**
- Time from "ticket ready" to "deploy" (median or P80).
- Optional: breakdown by stage (plan → implementation → review → QA → release).

**Scale 1–10 (guide):**
- 1–3: multi-week timelines with high variance.
- 4–6: days-to-week timelines with moderate variance.
- 7–10: day-level timelines with low, stable P80.

---

### 3.3 Quality and Rework (Defect Escape Rate)

**What it measures:** whether the architecture and agent reduce bugs or just move them around.

**What to collect:**
- Bugs in production per PR or per release.
- % of PRs requiring a "follow-up fix" within 7 days.
- Reverts or hotfixes per month.

**Scale 1–10 (guide):**
- 1–3: many escapes and reactive corrections.
- 4–6: acceptable quality with spikes.
- 7–10: low escape rate, infrequent corrections.

---

### 3.4 Decision Coherence (Consistency / Drift)

**What it measures:** whether the architecture maintains standards or gradually erodes over time.

**What to collect:**
- % of PRs that pass the architectural checklist (linters, conventions, ADRs).
- Number of "style/architecture" comments per PR (goal: trending down).
- Violations detected by tooling (lint, static analysis).

**Scale 1–10 (guide):**
- 1–3: every PR feels different.
- 4–6: standards exist but are frequently broken.
- 7–10: high consistency and few manual reminders.

---

### 3.5 Real Autonomy (Human-in-the-Loop Load)

**What it measures:** how much depends on the human vs. how much the flow "self-serves".

**What to collect:**
- Human time per PR (planning + corrections + guidance).
- Number of back-and-forth iterations with the agent per task.
- % of tasks where the "plan → PR ready" flow completes with few interventions (define N, e.g., ≤ 3).

**Scale 1–10 (guide):**
- 1–3: the human dictates everything.
- 4–6: the agent progresses but needs heavy guidance.
- 7–10: the human only validates and decides.

---

### 3.6 Cost and Efficiency (Cost Efficiency)

**What it measures:** whether the speed gains compensate for the cost (time, tools, tokens if applicable).

**What to collect:**
- Cost per ticket or per PR (if tracked).
- Total agent execution time per task (if available).
- Cost per "successful merge" (including failed attempts or retries), if applicable.

**Scale 1–10 (guide):**
- 1–3: expensive and unpredictable.
- 4–6: reasonable cost with waste.
- 7–10: stable cost and low waste.

---

## Recommended Use of the Quantification Framework

- **Pilot:** at close, fill in the six pillars with available data (even if approximate). Useful for communicating results and deciding what to improve.
- **Tracking:** every sprint or every 2–4 weeks, review at least 2–3 pillars (e.g., throughput, quality, autonomy) to detect trends without overhead.
- **Retro and improvement:** use the pillars as a guide in the retro ("did we go up or down in coherence? in autonomy?") and connect with the feedback changelog (section 2.4).
