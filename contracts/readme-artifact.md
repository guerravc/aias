# Artifact Contract — Cursor Configuration System (v2)

This document defines the **design contract** for task artifacts used in the rho-aias development architecture.

It exists to:
- Define what makes an artifact contract-compliant
- Establish naming conventions, directory rules, and TASK_ID format
- Provide quality criteria for reviewing artifact implementations
- Delegate runtime details (catalog, profiles, status lifecycle) to the `rho-aias` skill

This document is written **for maintainers** of the Cursor configuration system.

It is **not** a skill.
It is **not** executed by Cursor directly.
It is the reference against which artifact *implementations* are designed, reviewed, and corrected.

---

## Source of Truth

The **`rho-aias` skill** (`aias/.skills/rho-aias/`) is the single source of truth for all runtime artifact details:

| What | Where |
|------|-------|
| Closed artifact catalog (types, suffixes, producers) | `SKILL.md` — ARTIFACT CATALOG |
| Directory structure and discovery rules | `SKILL.md` — Discovery rules |
| Loading protocol (7 phases) | `SKILL.md` — LOADING PROTOCOL |
| Plan Classification (A/B/C) | `SKILL.md` — CORE RULES |
| Per-mode artifact requirements | `reference.md` — Per-Mode Artifact Requirements |
| Loading order | `reference.md` — Loading Order |
| Writing rules | `reference.md` — Writing Rules |
| Workflow profiles (steps, modes, chats) | `reference.md` — Workflow Profiles |
| Step definitions | `reference.md` — Step Definitions |
| `status.md` format and lifecycle | `reference.md` — status.md Format |
| Artifact sync states | `reference.md` — Artifact Sync Status |
| Structured Prompt fields | `reference.md` — Structured Prompt |
| Knowledge sync details | `reference.md` — Knowledge Sync Details |
| Directory state examples | `examples.md` |

This contract does **not** duplicate that content. When reviewing or creating artifacts, always consult the skill files for current values.

---

## What is an Artifact?

An **artifact** is a structured Markdown document that captures a specific aspect of a development task. Artifacts live in a centralized task directory and are produced and consumed by commands throughout the development lifecycle.

### Characteristics

- **Task-scoped** — Every artifact belongs to exactly one task directory
- **Type-constrained** — Only types in the closed catalog are allowed (see `SKILL.md`)
- **Suffix-discoverable** — Artifacts are found by globbing their suffix, not by hardcoding names
- **Sync-tracked** — Every artifact has a sync status relative to the resolved knowledge provider (`created` / `synced` / `modified`)

---

## Design Rules

### Naming Convention

All content artifacts follow the pattern:

```
<name>.<suffix>.md
```

- `<name>` — Descriptive label (e.g., `technical`, `analysis`, `report`, `delivery`, `instrumentation`, `delta`, `feasibility`)
- `<suffix>` — Type identifier used for glob discovery (e.g., `.plan`, `.product`, `.issue`, `.fix`, `.assessment`)
- `.md` — All artifacts are Markdown

`status.md` is the only exception — it has no suffix because it is a system file, not a content artifact.

### TASK_ID Format

- If a tracker ticket exists: use the ticket key exactly (e.g., `MAX-12345`)
- If no tracker ticket: use a descriptive kebab-case name (e.g., `refactor-auth-module`)

### Path Rules

- Base directory: resolved tasks directory (default: `~/.cursor/plans/`; configurable via `stack-profile.md` `binding.generation.tasks_dir` — see `aias/.skills/rho-aias/reference.md` § Tasks Base Directory)
- All artifacts for a task go in a single subdirectory: `<tasks_dir>/<TASK_ID>/`
- Old paths (`~/.cursor/issues/`, `~/.cursor/fixes/`, `~/.cursor/charters/`) are **deprecated**
- Never write artifacts outside TASK_DIR

### Catalog Rules

- The artifact catalog is **closed** — no new types may be invented by agents or commands
- Adding a new artifact type requires updating `SKILL.md`, `reference.md`, `examples.md`, and this contract
- Every artifact type must have: a unique suffix, a single producer command, and a clear purpose

---

## Quality Criteria

An artifact implementation is contract-compliant if:

1. File name matches the catalog in `SKILL.md` exactly
2. File lives inside `<resolved_tasks_dir>/<TASK_ID>/`
3. Suffix enables correct glob discovery
4. `status.md` is updated when the artifact is written (Phase 5)
5. Artifact sync status is tracked (`created` / `synced` / `modified`)
6. No artifact types exist outside the closed catalog
7. Content is written in English using **technical-writing** skill patterns

---

## References

- `aias/.skills/rho-aias/SKILL.md` — Artifact catalog, loading protocol, classification, core rules
- `aias/.skills/rho-aias/reference.md` — Per-mode requirements, profiles, step definitions, status.md format
- `aias/.skills/rho-aias/examples.md` — Directory states, status.md evolution examples
- `aias/contracts/readme-tracker-status-mapping.md` — Canonical tracker status mapping contract
