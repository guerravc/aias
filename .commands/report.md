# Bug Fix Report (Ticket Companion) — v2

## 1. Identity

**Command Type:** Type B — Procedural / Execution

You are generating a **bug fix report** to be attached to a ticket (ticket companion **after** root cause or fix have been validated). If they are not yet validated, the command must warn or refuse. Use a minimal template: Bug Summary, Expected vs Actual, Root Cause, Resolution. Typically used when closing a bug.

**Skills referenced:** `rho-aias`, `technical-writing`.

---

## 2. Invocation / Usage

Invocation:
- `/report`

Usage notes:
- This command is intended to be used **after** a reasoning or debugging step (e.g. `@debug`, `@qa`).
- It must not be invoked if the root cause or fix has not been validated.

---

## 3. Inputs

This command may use **only** the following inputs:
- Chat context explicitly provided by the user
- Output from a prior reasoning step (e.g. `@debug`, `@qa`)
- Artifacts from TASK_DIR loaded via **rho-aias** skill (Phases 0–3) if TASK_DIR is set: `report.issue.md`, `analysis.fix.md`
- Logs, diffs, or notes explicitly pasted by the user

Rules:
- All inputs must be explicit.
- If required information is missing, the command must request it before producing output.

---

## 4. Output Contract (Format)

- **Default:** The response **MUST** be returned as **a single Markdown code block** using ```markdown (output only in chat).
- If the user **explicitly** asks to publish to the ticket (e.g. "publish to the ticket", "post as comment"), fire the Artifact Preview gate first, then resolve the tracker provider and publish.
- If tracker config is missing, invalid, or points to an unresolvable provider, abort publish and request provider configuration. Otherwise output only in chat.

### Gate: Artifact Preview

**Type:** Confirmation
**Fires:** Before publishing the report to the resolved tracker provider (only when the user explicitly requests tracker publish).
**Skippable:** No.

**Context output:**
Present publish preview in chat:
- Report content summary (bug title, root cause, fix applied)
- Target ticket ID
- Tracker provider action (post as comment)

**AskQuestion:**
- **Prompt:** "Publish bug fix report to <TICKET_ID> via tracker provider?"
- **Options:**
  - `publish`: "Publish to tracker"
  - `adjust`: "Adjust content before publishing"
- **allow_multiple:** false

**On response:**
- `publish` → Publish via resolved tracker provider
- `adjust` → Apply corrections, return to context output and re-present gate

**Anti-bypass:** Inherits Gate Invocation Protocol. No additional rules.
- The code block **MUST start** with a Markdown heading (`##`). Only one code block is allowed. Do not write to a file unless the contract defines it.

SERVICE RESOLUTION PSEUDOFLOW:

```
resolveTrackerProvider():
  if tracker-config exists and is valid:
    use active_provider + skill_binding + provider parameters
  else:
    abort and request tracker service configuration
```

---

## 5. Content Rules (Semantics)

- Output must be in **English**.
- Apply the **technical-writing** skill patterns: Problem Framing, Root Cause Description, Conciseness.
- Do **NOT** invent information.
- Use **ONLY** the provided inputs.
- If information is missing, leave the section empty or include a short placeholder comment.
- Focus on **causality and resolution**, not on implementation details.
- Do **NOT** include large code blocks unless explicitly provided in the input.

---

## 6. Output Structure (Template)

```markdown
## Bug Summary
<!-- Short description of the observed problem -->

## Expected vs Actual Behavior
**Expected:**
<!-- What should have happened -->

**Actual:**
<!-- What actually happened -->

## Root Cause
<!-- The underlying cause of the bug (not just the symptom) -->

## Before (Problem State)
<!-- Brief description of the problematic behavior -->
<!-- Do not paste large code blocks -->

## Fix Applied
<!-- What was changed to resolve the issue -->

## After (Result)
<!-- How the system behaves after the fix -->

## Regression Prevention
<!-- Tests, guards, or invariants added (if any) -->
```

---

## 7. Non-Goals / Forbidden Actions

This command must **NOT**:
- Infer root cause or resolution from symptoms alone; only structure what comes from context (e.g. @debug or validated /fix)
- Publish to the ticket unless the user explicitly asks
- Perform analysis or debugging
- Infer missing context
- Write or modify code
- Touch files or repositories
- Suggest alternative fixes
- Execute commands or scripts

