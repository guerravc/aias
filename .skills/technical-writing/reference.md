# Technical Writing — Pattern Reference

Detailed rules, anti-patterns, and checklists for each writing pattern.

---

## 1. Problem Framing

### Formula

**Context → Impact → Current behavior vs Desired behavior**

Structure:
1. **Context** (1 sentence): What area of the system or user experience is affected.
2. **Impact** (1 sentence): Who is affected and how severely (users, developers, business).
3. **Current vs Desired** (1–2 sentences): What happens today vs what should happen.

### Rules

- Max one paragraph (3–5 sentences).
- Name the actor (user, system, API, scheduler) — never say "it" without a clear antecedent.
- Quantify when possible ("loads in 8s" not "loads slowly").
- Separate the problem from the solution — do not propose fixes here.

### Anti-Patterns

- **Vague opener:** "There is an issue with the login flow." → Say what the issue IS.
- **Solution disguised as problem:** "We need to add caching." → State the performance problem first.
- **Missing impact:** Describing behavior without saying why it matters.
- **Compound problems:** Mixing two unrelated issues in one framing. Split them.

### Checklist

- [ ] Can a reader understand the problem without prior context?
- [ ] Is the impact explicit (who, how severe)?
- [ ] Is current vs desired behavior stated, not implied?
- [ ] Is it free of proposed solutions?

---

## 2. Acceptance Criteria

### Formula

**Given [precondition], When [action], Then [observable outcome].**

Each criterion is one testable statement. Group related criteria under a single feature heading if needed, but each criterion must stand alone.

### Rules

- Every criterion must be **independently verifiable** — a tester can confirm pass/fail.
- Use observable outcomes ("displays error message X", "returns HTTP 400"), not internal states ("sets flag to true") unless testing internals is the goal.
- Include both happy path and key failure paths.
- Max 1 sentence per criterion. If it needs more, it's two criteria.

### Anti-Patterns

- **Untestable:** "The feature works correctly." → Define what "correctly" means.
- **Vague outcome:** "Handles edge cases." → Name the specific edge cases.
- **Implementation detail:** "Uses a dictionary to store values." → Describe the behavior, not the mechanism.
- **Missing negative case:** Only describing what happens when things go right.

### Checklist

- [ ] Can each criterion be verified with a pass/fail result?
- [ ] Are outcomes observable (UI state, API response, error message)?
- [ ] Are happy path AND key failure paths covered?
- [ ] Is each criterion one sentence?

---

## 3. Root Cause Description

### Formula

**Evidence → Mechanism → Consequence**

Structure:
1. **Evidence** (1 sentence): What was observed (log output, crash stack, wrong UI state, test failure).
2. **Mechanism** (1–2 sentences): The technical reason it happened (race condition, nil dereference, missing guard, wrong API contract).
3. **Consequence** (1 sentence): The user-visible or system-visible result.

### Rules

- Name the specific mechanism — never say "caused by a bug" or "due to an error."
- Link evidence to mechanism explicitly ("The crash log shows a nil unwrap at line 42, caused by the response parser not handling empty arrays").
- If the root cause is uncertain, state the hypothesis and what evidence would confirm it.

### Anti-Patterns

- **Circular:** "The crash happened because there was a crash in the parser." → Name the specific failure.
- **Skipping evidence:** Jumping to mechanism without stating what was observed.
- **Vague mechanism:** "Something went wrong with the network call." → Name the specific failure mode (timeout, 401, DNS resolution, malformed JSON).
- **Missing consequence:** Explaining the mechanism without connecting it to user impact.

### Checklist

- [ ] Is the evidence concrete (log line, stack trace, screenshot, test output)?
- [ ] Is the mechanism specific (named failure mode, not "a bug")?
- [ ] Is the consequence connected to user or system impact?
- [ ] If uncertain, is the hypothesis explicitly labeled?

---

## 4. Risk Articulation

### Formula

**[What could happen] + [Likelihood: low/medium/high] + [Impact: low/medium/high] + [Mitigation or acceptance]**

One sentence per risk. Can be formatted as a table for multiple risks.

### Rules

- Every risk must have a mitigation OR explicit acceptance ("accepted: low likelihood, recovery is straightforward").
- Likelihood and impact are relative to the project context, not absolute.
- Be specific about "what could happen" — not "things could go wrong" but "the migration could fail mid-batch, leaving data in an inconsistent state."
- Separate risks from issues. A risk is something that **might** happen. An issue is something that **has** happened.

### Anti-Patterns

- **Unactionable:** "There are risks associated with this change." → Name them.
- **Missing mitigation:** "This could break the build." → Add what to do about it.
- **Inflated likelihood:** Everything marked "high" loses signal. Calibrate honestly.
- **Risk as task:** "We need to test performance." → That's a task, not a risk. The risk is "response time could exceed 3s under load."

### Checklist

- [ ] Is each risk specific (named scenario, not vague concern)?
- [ ] Does each risk have likelihood + impact?
- [ ] Does each risk have mitigation or explicit acceptance?
- [ ] Are risks separated from issues and tasks?

---

## 5. Increment Goals

### Formula

**One sentence: [Verb] + [what is achieved] + [how to verify].**

Examples of the formula:
- "Implement the auth token refresh flow; verified by unit test passing and manual login/logout cycle."
- "Replace hardcoded strings with localization keys; verified by all UI labels rendering correctly in English."

### Rules

- Starts with an action verb (implement, add, replace, refactor, remove, migrate).
- Must be verifiable — include how to confirm "done" (test passes, UI renders, API returns expected response).
- Self-contained — no dependency on future increments. If this increment is the last one completed, the system must still work.
- One sentence. If it needs two, the increment is too big — split it.

### Anti-Patterns

- **Vague:** "Work on the UI." → Name what specifically changes in the UI.
- **Unverifiable:** "Improve the architecture." → State what changes and how to confirm.
- **Dependent:** "Prepare the data layer for the next increment." → Each increment must deliver value alone.
- **Multi-goal:** "Implement auth flow and refactor the network layer." → Two increments.

### Checklist

- [ ] Starts with an action verb?
- [ ] Includes verification method?
- [ ] Self-contained (no dependency on future increments)?
- [ ] One sentence?

---

## 6. Conciseness Rules

### Words and Phrases to Remove

| Remove | Replace with |
|---|---|
| "In order to" | "To" |
| "Basically" / "Essentially" | (delete) |
| "It should be noted that" | (delete, state the thing directly) |
| "Due to the fact that" | "Because" |
| "At this point in time" | "Now" |
| "In the event that" | "If" |
| "Prior to" | "Before" |
| "Subsequent to" | "After" |
| "Utilize" | "Use" |
| "Facilitate" | "Help" / "Enable" |
| "Leverage" | "Use" |
| "Implement a solution for" | "Fix" / "Solve" |

### Rules

- Prefer active voice when the actor is known ("The parser fails" not "A failure is encountered by the parser").
- Passive voice is acceptable when the actor is irrelevant or unknown ("The token is refreshed every 30 minutes").
- One idea per sentence. If a sentence has "and" connecting two unrelated ideas, split it.
- No hedging without purpose. "This might possibly cause issues" → "This risks causing [specific issue]" or remove if the risk is not worth stating.

### Anti-Patterns

- **Filler paragraphs:** Opening with "It is important to note that..." before the actual content.
- **Redundant qualifiers:** "Completely finished", "Absolutely required", "Totally unique."
- **Apologetic tone:** "This is just a suggestion, but maybe we could..." → State the recommendation directly.
- **Throat-clearing:** Multiple sentences of context before the actual point. Lead with the point.

### Checklist

- [ ] Can any sentence be shortened without losing meaning?
- [ ] Is active voice used when the actor is known?
- [ ] Is each sentence one idea?
- [ ] Are filler words and phrases eliminated?
