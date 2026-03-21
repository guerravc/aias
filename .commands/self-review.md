# Self Review (Own-Work Review) — v1

## 1. Identity

**Command Type:** Type A — Chat-only / Analysis

You are reviewing the user's own implementation work using the `@review` reasoning stance. This command is for **self-review of local work** before PR creation or before requesting external review.

**Skills referenced:** `rho-aias`.

---

## 2. Invocation / Usage

Invocation:
- `/self-review`
- `/self-review <TASK_ID>`

Usage notes:
- Best used with `@review` mode active.
- Prefer this command when the primary context is local code plus local artifacts in TASK_DIR.
- This command produces findings in chat only. It does not write artifacts, publish to providers, or modify code.

---

## 3. Inputs

This command may use **only** the following inputs:
- Modified code in the current repository / working tree
- TASK_DIR artifacts loaded via `rho-aias` when available: `dod.plan.md`, `increments.plan.md`, `technical.plan.md`, `dor.plan.md`, `specs.design.md` (when present)
- Chat context explicitly provided by the user

Rules:
- If TASK_DIR is available, load it and prioritize local artifacts as review intent context.
- If TASK_DIR is not available, the command MAY still review the modified code, but MUST state the missing planning context.
- `dod.plan.md` SHOULD be treated as the primary intent/checklist artifact when present.

---

## 4. Output Contract (Format)

Return findings directly in chat, prioritized by severity:

1. Blocking issues
2. Major risks
3. Minor improvements
4. Open questions / assumptions
5. Overall readiness

Each finding SHOULD tie observed code back to one or more of:

- `dod.plan.md`
- `increments.plan.md`
- `technical.plan.md`
- local coding standards and architecture

If no findings exist, say so explicitly and state any residual testing or context gaps.

---

## 5. Content Rules (Semantics)

- Output must be in **English**.
- Review modified code only.
- Do **NOT** implement fixes unless the user explicitly asks.
- MUST distinguish blocking issues from optional improvements.
- SHOULD evaluate alignment between actual code, `dod.plan.md`, and increment goals.
- If local artifacts and code conflict, report the drift explicitly.

---

## 6. Review Procedure

1. Resolve TASK_DIR when available via `rho-aias`.
2. Load local review context in this priority order:
   - `dod.plan.md`
   - `increments.plan.md`
   - `technical.plan.md`
   - other relevant local artifacts
3. Inspect modified code and compare it against local plan intent.
4. Report findings with evidence and impact.
5. Conclude whether the work looks ready for PR / peer review, or what should be addressed first.

---

## 7. Non-Goals / Forbidden Actions

This command must **NOT**:
- Create or update artifacts
- Publish to knowledge provider
- Post to tracker or VCS provider
- Replace `@review` with implementation behavior
- Pretend a PR review happened when only local work was reviewed
