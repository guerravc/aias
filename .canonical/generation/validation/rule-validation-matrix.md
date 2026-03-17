# Rule Validation Matrix — Phase 4: Rule Abstraction

> Validates that workspace rules are contract-compliant, drift-free, and deterministically maintainable after Phase 4 normalization.

## Execution Instructions

1. For each check, apply the Method to the specified Scope.
2. Record the Observed Evidence (file path, line, or content).
3. Mark Status as Pass/Fail/Gap.
4. For any Fail/Gap, add a Remediation Action and assign an Owner.

## Pass Threshold

- **All checks must pass** for Phase 4 closure.
- Gaps require documented remediation plans with owners before closure.

## Gap Triage Policy

| Severity | Criteria | Action |
|---|---|---|
| Blocking | Invariant violation, provider hardcoding, contract non-compliance | Fix immediately before closure |
| Non-blocking | Justified deviation in specialized workspace | Document in Exception Registry below |
| Informational | Minor style difference, no behavioral impact | Note for future improvement |

---

## Validation Checks

| Check ID | Scope | Rule Aspect | Method | Expected Result | Observed Evidence | Status |
|---|---|---|---|---|---|---|
| RV-001 | Global | Invariant | Verify LANGUAGE section text is identical across all 6 base.mdc (post-normalization) | All 6 files use "Tracker tickets" (category-driven, no provider names) | All 6 base.mdc updated: "Jira tickets" → "Tracker tickets" | Pass |
| RV-002 | Global | Invariant | Verify COMMANDS AND SKILLS section is identical across all 6 base.mdc | Byte-identical content in all 6 files | All 6 files share identical 9-line COMMANDS AND SKILLS block | Pass |
| RV-003 | Global | Invariant | Verify CONFLICT HANDLING section is identical across all 6 base.mdc | Same 4-line structure in all files | Identical across all 6 | Pass |
| RV-004 | Global | Invariant | Verify CODE PRESERVATION exists in all 6 base.mdc | Present in all files | Present in all 6 (xctemplates-dev uses template-specific wording — see EX-001) | Pass |
| RV-005 | Global | Invariant | Verify SECURITY exists in all standard base.mdc (xctemplates-dev exempted — EX-001) | Present in all standard workspaces | Present in all standard workspaces per onboarded platforms | Pass |
| RV-006 | Global | Parametrized | Verify each ROLE section is workspace-specific and non-generic | Each workspace has a distinct, specialized role | Distinct roles verified per onboarded workspace | Pass |
| RV-007 | Global | Parametrized | Verify STYLE section references workspace-specific styleguides | Each workspace references its own styleguide paths | 6 distinct STYLE sections verified, matching stack profile bindings | Pass |
| RV-008 | Global | Parametrized | Verify LIMITATIONS & TRUTHFULNESS references platform-appropriate toolchain | Each workspace references its own platform toolchain | Each workspace verified against stack profile bindings | Pass |
| RV-009 | Profile | Parametrized | Verify domain-specific constraint sections exist only where justified | rdsui-dev: DESIGN SYSTEM, rdsnetworking-dev: NETWORKING, rdsmacros: MACRO, xctemplates: TEMPLATE | All 4 domain-specific sections present and justified | Pass |
| RV-010 | Global | Compatibility | Verify output-contract.mdc frontmatter uses alwaysApply: true | All 6 files | All 6 confirmed | Pass |
| RV-011 | Global | Compatibility | Verify output-contract.mdc has ENVIRONMENT section | All 6 files | All 6 have ENVIRONMENT as first content section | Pass |
| RV-012 | Global | Compatibility | Verify output-contract.mdc has DELIVERABLES section with 3-part structure | All 6 files | All 6 follow: 1) Reasoning 2) Code changes 3) Documentation | Pass |
| RV-013 | Profile | Compatibility | Verify output-contract profile assignment matches workspace type | Each workspace profile matches its stack profile declaration | All assignments correct per stack profile bindings | Pass |
| RV-014 | Global | Propagation | Verify stack profiles declare all required binding.rule.base.* keys | All onboarded platforms × workspaces × required keys | All required keys present in all stack profiles | Pass |
| RV-015 | Global | Propagation | Verify stack profiles declare all required binding.rule.output_contract.* keys | Per-workspace profile and environment bindings | All required keys present | Pass |
| RV-016 | Global | Invariant | Verify no provider hardcoding remains in any base.mdc | grep for "Jira", "Confluence", "GitHub", "Figma" in text content (not in example command references) | Post-audit remediation (F-001): "Confluence sync, and Jira sync" → "knowledge-provider sync, and tracker-provider sync"; "archives all task artifacts to Confluence" → "archives all task artifacts to the resolved knowledge provider". Fixed in template + 7 base.mdc (6 generated + 1 manual xctemplates-dev). | Pass (remediated) |
| RV-017 | Contract | Compatibility | Verify readme-output-contract.md exists and is referenced | File at aias/contracts/readme-output-contract.md | Created in Increment 1 | Pass |
| RV-018 | Contract | Compatibility | Verify readme-base-rule.md includes invariant/parametrizable taxonomy | Section "Invariant vs Parametrizable Sections" exists | Added in Increment 1 | Pass |
| RV-019 | Contract | Compatibility | Verify readme-stack-profile.md includes Rule Bindings section | Section "Rule Bindings" with base and output_contract binding tables | Added in Increment 1 | Pass |
| RV-020 | Global | Propagation | Verify canonical sources exist in aias/.canonical/ | base-rule.md and output-contract.md | Both created in Increment 2 (renamed from *-template.md in BL-S08) | Pass |

---

## Exception Registry

| ID | Workspace | Deviation | Justification | Owner |
|---|---|---|---|---|
| EX-001 | xctemplates-dev | Missing SECURITY, PERFORMANCE sections; CODE PRESERVATION uses template-specific wording; ENGINEERING PRINCIPLES are template-focused (no SOLID reference) | xctemplates-dev develops Xcode file templates, not application/library code. Standard app development sections (SECURITY, PERFORMANCE) add no value. Template-specific CODE PRESERVATION and ENGINEERING PRINCIPLES are more appropriate. | Maintainer |
| EX-002 | xctemplates-dev | STYLE section is simplified (3 lines vs multi-reference format) | No project-level styleguide files in template repos; style rules are inline | Maintainer |

---

## Acceptance Criteria Traceability

| Acceptance Criterion (from Intent) | Check IDs | Status |
|---|---|---|
| Focused workspaces use consistent canonical rule behavior with explicit parameterization | RV-001 through RV-009, RV-014, RV-015 | Pass |
| Rule drift across focused workspaces is reduced and auditable | RV-016, RV-020, Exception Registry | Pass |
| Contracts remain compliant after normalization | RV-010 through RV-013, RV-017 through RV-019 | Pass |
| Rule updates can be applied once and propagated deterministically | RV-014, RV-015, RV-020 | Pass |
