# Contract Versioning Policy — Rho AIAS Configuration System (v1.0)

> **Keyword convention**: This contract uses RFC-2119 keywords (MUST, MUST NOT, SHOULD, MAY).
> See [readme-commands.md](readme-commands.md) § RFC-2119 Keyword Policy for definitions.

This document defines the canonical policy for versioning, deprecation, and backward compatibility of contracts in the Rho AIAS configuration system.

It exists to:
- Establish a uniform versioning scheme across all contracts
- Define when and how contract versions MUST be bumped
- Provide deprecation rules with minimum coexistence periods
- Enable adopters and contributors to assess breaking changes

This document is written **for maintainers** of the Rho AIAS configuration system.

---

## Versioning Scheme

All contracts use a **Major.Minor** scheme:

| Level | When to bump | Examples |
|---|---|---|
| **Major** (v1 -> v2) | Breaking change: removes or renames a mandatory section, changes semantics of an existing requirement, removes a mandatory binding key, alters the normative schema | Removing a mandatory section, renaming `resource_files` to `dependencies` |
| **Minor** (v1.0 -> v1.1) | Additive or clarifying change: new optional section, new optional binding key, editorial rewording that does not alter normative meaning, adding examples | Adding a new optional field, clarifying an ambiguous rule |

Patch versions (v1.0.1) are NOT used. Editorial fixes that do not change normative meaning (typos, formatting) do not require a version bump.

---

## Version Placement

Every contract MUST declare its version in the H1 heading:

```
# <Contract Name> — <System Name> (v<Major>.<Minor>)
```

When a contract has no prior version history, it SHOULD be initialized at **v1.0**.

---

## Current Contract Versions

| Contract | Current Version |
|---|---|
| `readme-artifact.md` | v2.0 |
| `readme-base-rule.md` | v1.0 |
| `readme-commands.md` | v6.0 |
| `readme-knowledge-publishing-config.md` | v1.0 |
| `readme-mode-rule.md` | v1.0 |
| `readme-output-contract.md` | v1.0 |
| `readme-project-context.md` | v1.0 |
| `readme-provider-config.md` | v2.1 |
| `readme-skill.md` | v1.1 |
| `readme-stack-profile.md` | v1.2 |
| `readme-tool-adapter.md` | v1.0 |
| `readme-tracker-field-mapping.md` | v1.0 |
| `readme-tracker-status-mapping.md` | v1.0 |
| `readme-versioning-policy.md` | v1.0 |

---

## Deprecation Policy

When a contract element (section, key, field, behavior) is deprecated:

1. **Mark in contract**: Add a `**deprecated**` annotation with the version that introduced the deprecation and the replacement (if any).
2. **CHANGELOG entry**: Record the deprecation in `aias/CHANGELOG.md` under the framework version that ships the change.
3. **Minimum coexistence**: The deprecated element MUST remain functional for at least **one major framework version** after the deprecation notice. Example: if deprecated in v8.x, removal is allowed starting in v9.0.
4. **Removal**: When removing a deprecated element, bump the contract's major version and document the removal in the CHANGELOG.

### Deprecation Notice Format

```markdown
- `<element>` — **deprecated** (v<contract_version>). Use `<replacement>` instead. Scheduled for removal in framework v<N>.0.
```

---

## Backward Compatibility Rules

1. **Additive changes are safe**: New optional sections, new optional keys, and new examples do not break existing consumers.
2. **Normative changes require major bump**: Changing the meaning of MUST/SHOULD/MAY for an existing requirement is a breaking change.
3. **Removal requires deprecation first**: No mandatory element MAY be removed without a prior deprecation period.
4. **Rename = remove + add**: Renaming a mandatory key or section is treated as removal of the old and addition of the new — requires major version bump and deprecation of the old name.

---

## When to Bump

| Scenario | Action |
|---|---|
| Fix a typo in descriptive text | No bump |
| Reword a normative rule without changing meaning | No bump |
| Add a new optional section | Minor bump |
| Add a new optional binding key | Minor bump |
| Add examples or clarifying notes | Minor bump |
| Change a SHOULD to a MUST | Major bump |
| Remove a mandatory section | Major bump (+ prior deprecation) |
| Rename a mandatory key | Major bump (+ prior deprecation of old key) |
| Add RFC-2119 preamble | No bump (editorial) |

---

## Relationship with Framework Versions

Contract versions are independent of framework versions (`v8.0`, `v9.0`, etc.). A framework version release MAY include changes to zero or more contracts. The CHANGELOG records both:
- Framework-level milestones (in the Release History table)
- Contract-level changes (mentioned within the relevant framework version entry)

---

## Quality Criteria

A properly versioned contract:

1. Has a version number in the H1 heading
2. Bumps version on every normative change
3. Uses deprecation notices before removing elements
4. Is reflected in the Current Contract Versions table above
5. Has its version changes recorded in `aias/CHANGELOG.md`

---

## Related Contracts

- `readme-commands.md` — RFC-2119 Keyword Policy (canonical definition)
- `aias/CONTRIBUTING.md` — PR guidelines referencing contract versions

---

This document is the **source of truth** for contract versioning and deprecation policy.
