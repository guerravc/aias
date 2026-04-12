# Progressive Adoption Guide

This guide provides a structured path for adopting Rho AIAS incrementally. You do not need to learn or configure everything on day one. Start with the minimum viable setup and expand as your workflow matures.

---

## Level 1 — Minimum Viable Adoption (Day 1)

**Goal:** Start producing structured plans and implementations immediately after `aias init`.

### What you activate

| Component | Items |
|-----------|-------|
| **Modes** | `@planning`, `@dev` |
| **Commands** | `/blueprint`, `/implement`, `/commit` |
| **Providers** | None required |

### What you configure

1. Run `aias init` — this creates `RHOAIAS.md`, `stack-profile.md`, `stack-fragment.md`, and generates shortcuts.
2. No external services needed. All operations are local.

### What you get

- Structured planning: `/blueprint` collects requirements and writes `technical.plan.md` + `increments.plan.md`.
- Plan-driven implementation: `/implement` executes increments with feedback gates.
- Conventional commits: `/commit` stages, formats, and commits changes.

### Workflow

```
@planning + /blueprint → @dev + /implement → /commit
```

### When to move to Level 2

- You need to share artifacts with your team (Confluence, Jira).
- You want automated tracker transitions.
- You start working on features that need refinement before planning.
- You want code review assistance.

---

## Level 2 — Team Workflow (Week 1)

**Goal:** Integrate team collaboration, tracker sync, and quality gates.

### What you add

| Component | Items |
|-----------|-------|
| **Modes** | `@product`, `@review`, `@qa` |
| **Commands** | `/enrich`, `/validate-plan`, `/pr`, `/self-review`, `/peer-review` |
| **Providers** | `tracker` (Jira), `knowledge` (Confluence) |

### What you configure

1. Create provider configs: `aias-config/providers/tracker-config.md` and `knowledge-config.md` (via `/aias configure-providers` or `aias new --provider`).
2. Set up provider-specific documents: `tracker-status-mapping.md`, `jira-field-mapping.md`, `confluence-config.md` under `aias-config/providers/<provider_id>/`.
3. Verify with `aias health`.

### What you get

- **Refinement workflow:** `/enrich` produces `analysis.product.md`, `dor.plan.md`, `dod.plan.md` — Definition of Ready and Done before planning.
- **Plan validation:** `/validate-plan` audits your plan against DoR/DoD and flags gaps.
- **Tracker sync:** Status transitions fire automatically (`pending_dor` → `ready` → `in_progress` → `in_review`).
- **Knowledge publishing:** Phase 5c publishes artifacts to Confluence after every command.
- **Code review:** `/self-review` and `/peer-review` provide structured review analysis.
- **Pull requests:** `/pr` creates PRs with plan delta sections.

### Workflow

```
@product + /enrich → @planning + /blueprint → /validate-plan → @dev + /implement → /self-review → /commit → /pr
```

### When to move to Level 3

- You work on high-impact features that need governance gates (approval before implementation).
- You need design provider integration (Figma).
- You want full lifecycle closure with `/publish`.
- You manage cross-team or architectural changes.

---

## Level 3 — Full Governance (Month 1)

**Goal:** Full SDLC coverage with classification-based governance, design integration, and lifecycle closure.

### What you add

| Component | Items |
|-----------|-------|
| **Modes** | `@debug`, `@integration`, `@delivery`, `@devops` |
| **Commands** | `/publish`, `/consolidate-plan`, `/charter`, `/guide`, `/explain`, `/handoff`, `/report`, `/brief`, `/assessment`, `/fix`, `/issue`, `/trace`, `/run`, `/test`, `/spm`, `/copyedit` |
| **Providers** | `design` (Figma), `vcs` (GitHub) |
| **Governance** | Plan Classification (Minor / Standard / Critical) |

### What you configure

1. Add remaining provider configs: `design-config.md` and `vcs-config.md`.
2. Understand Plan Classification:
   - **Minor** — Small, single-increment changes. No custom governance gates.
   - **Standard** — Medium-impact features. Optional custom governance gates.
   - **Critical** — Large architectural changes. Mandatory approval gate before implementation.
3. Verify full setup with `aias health`.

### What you get

- **Full governance:** Classification determines which gates fire in `/implement`. Critical plans require explicit approval.
- **Design integration:** `/blueprint` fetches design specs from Figma automatically.
- **Bug workflows:** `/issue` → `/trace` → `/fix` → `/assessment` → `/blueprint` for structured debugging.
- **Lifecycle closure:** `/publish` reconciles all artifacts and generates the plan delta.
- **Charter escalation:** `/charter` can escalate classification (minor→standard, standard→critical).
- **Cross-chat continuity:** `/handoff` produces context transfer snippets.

### Full workflow (feature)

```
@product + /enrich → @planning + /blueprint → /validate-plan → @dev + /implement → /self-review → /commit → /pr → /publish
```

### Full workflow (bugfix)

```
@qa + /issue → @debug + /trace → @dev + /fix → /assessment → @planning + /blueprint → @dev + /implement → /commit → /pr → /publish
```

---

## Summary Table

| Capability | Level 1 | Level 2 | Level 3 |
|------------|---------|---------|---------|
| Local planning + implementation | Yes | Yes | Yes |
| Conventional commits | Yes | Yes | Yes |
| Refinement (DoR/DoD) | — | Yes | Yes |
| Tracker sync | — | Yes | Yes |
| Knowledge publishing | — | Yes | Yes |
| Code review | — | Yes | Yes |
| Pull requests | — | Yes | Yes |
| Plan Classification governance | — | — | Yes |
| Design integration | — | — | Yes |
| Bug workflows | — | — | Yes |
| Lifecycle closure | — | — | Yes |
| Charter escalation | — | — | Yes |

---

## Related Documentation

- [Quick Start Guide](QUICKSTART.md) — Setup and first feature tutorial (Level 1)
- [Configuration Guide](CONFIGURATION.md) — Detailed configuration reference
- [Workflows](WORKFLOWS.md) — End-to-end workflow descriptions
- [Architecture](ARCHITECTURE.md) — Framework layer overview
