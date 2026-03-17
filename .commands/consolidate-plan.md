# Consolidate Plan (Gap-by-Gap Plan Strengthening) — v3

## 1. Identity

**Command Type:** Type B — Procedural / Execution

You are **consolidating** an implementation plan by working through gaps identified by `/validate-plan`, one at a time, using the plan artifacts in the task directory.
This command is responsible for guiding a strict workflow: one gap → one proposal → refinement until the user agrees → update the relevant artifact(s) in TASK_DIR only when explicitly instructed.

**Skills referenced:** `rho-aias`.

---

## 2. Invocation / Usage

Invocation:
- `/consolidate-plan`
- `/consolidate-plan <TASK_ID>`

Usage notes:
- Use **after** plan artifacts exist in TASK_DIR and `/validate-plan` has identified gaps.
- This command processes gaps reported by `/validate-plan` one at a time.
- Plan artifacts are loaded from TASK_DIR via the **rho-aias** skill loading protocol.
- If TASK_DIR is not set, ask the user for the task ID.

---

## 3. Inputs

This command may use **only** the following inputs:
- Plan artifacts from TASK_DIR loaded via **rho-aias** skill (Phases 0–3): `technical.plan.md`, `increments.plan.md`, `dor.plan.md`, `dod.plan.md`, `specs.design.md` (when present), `analysis.product.md` (when present)
- Gap list from `/validate-plan` output (in chat context or re-run `/validate-plan` to refresh)
- Chat context: the user's replies during the refinement cycle

Rules:
- All inputs must be explicit.
- If no gaps exist, report "No gaps to consolidate" and stop.
- The command must **not** assume which gap to start with unless the user specifies one; by default, take the first gap from the list.
- Each proposal must identify the **specific artifact file(s)** that will be modified (e.g., "update `dor.plan.md`", not just "update the plan").

---

## 4. Output Contract (Format)

- **Proposals** must be presented in the chat as plain Markdown. Structure: a clear heading for the gap, then the concrete changes (what to add, change, or remove in which artifact), then a short summary.
- **Refinement:** Replies in chat; no fixed template. The command adjusts the proposal based on user feedback until the user explicitly approves.
- **After artifact update:** Confirm in chat: which gap was resolved, which artifact(s) were updated, and list remaining gaps.
- **File output:** The command may write **only** to plan artifacts in TASK_DIR, and **only** when the user has explicitly instructed to update. After updating, run Phase 5 to mark artifacts as `modified` and sync to resolved knowledge provider.

---

## 5. Content Rules (Semantics)

- Output may be in **English** or **Spanish** depending on the user's language in the conversation.
- Do **not** invent gaps or changes; only work with reported gaps and the user's feedback.
- Proposals must be **concrete**: specify which artifact(s) (e.g. `dor.plan.md`, `increments.plan.md`) and what text or structure to add or change.
- Do **not** update artifacts until the user explicitly instructs to do so.

---

## 6. Output Structure (Procedure)

The command follows this **procedure**. Do not skip steps or update artifacts before the user instructs.

### Step 1 — Select current gap

- Follow **rho-aias** skill loading protocol (Phases 0–3) to load TASK_DIR artifacts.
- Identify gaps from `/validate-plan` output in chat context. If no gaps in context, ask the user to run `/validate-plan` first.
- Select **one** gap: the first in the list, or the one the user specified.
- Announce which gap is being worked on.

### Step 2 — Propose

- Produce a **proposal**: concrete changes to the relevant artifact(s) that address this gap.
- Present the proposal in chat. Do **not** modify artifacts yet.

### Step 3 — Refinement cycle

- MUST wait for the user's response before proceeding.
- If the user requests changes: revise the proposal. Repeat until the user indicates agreement.
- If the user cancels or switches gap: acknowledge and select another gap.
- When the user indicates agreement with the proposal, fire the Update Approval gate.

#### Gate: Update Approval

**Type:** Confirmation
**Fires:** Step 3, when the user agrees with a proposal and the command is about to write to artifact files.
**Skippable:** No.

**Context output:**
Present the approved proposal summary:
- Gap being resolved
- Artifact file(s) to update
- Summary of changes to apply

**AskQuestion:**
- **Prompt:** "Apply this proposal to <artifact-file(s)>?"
- **Options:**
  - `apply`: "Apply changes to artifact(s)"
  - `revise`: "Revise the proposal further"
- **allow_multiple:** false

**On response:**
- `apply` → Proceed to Step 4
- `revise` → Return to refinement cycle

**Anti-bypass:** Inherits Gate Invocation Protocol. No additional rules.

### Step 4 — Update artifacts

- Apply the **approved** proposal to the relevant artifact file(s) in TASK_DIR.
- Run Phase 5: mark updated artifacts as `modified` in `status.md`, sync to resolved knowledge provider.
- Confirm in chat: "Artifact updated. Gap resolved." and list remaining gaps.

### Step 5 — Next gap (optional)

- If there are more gaps and the user wants to continue, repeat from Step 1. Otherwise, the command run is complete.

---

## 7. Non-Goals / Forbidden Actions

This command must **NOT**:
- Update artifacts without the user's explicit instruction to do so.
- Work on more than one gap at a time.
- Invent gaps or add new content beyond what the gap requires.
- Execute external commands or scripts (except reading/writing artifacts in TASK_DIR).
- Perform deep reasoning beyond proposing and refining artifact text.
- Write artifacts outside TASK_DIR.
