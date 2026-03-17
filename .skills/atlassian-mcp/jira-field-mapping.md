# Jira Field Mapping — MAX Project (Story)

Field mapping for the MAX project, Story issue type (`issuetype.id: 10005`).
Used by `/enrich`, `/report`, and `/brief` when pushing content to Jira.

---

## 1. Traceability: Product → Enrich → Jira Field

How `@product` product analysis and `/enrich` dimensions map to Jira fields.

| @product output | /enrich dimension | Jira field | Field key | Format |
|---|---|---|---|---|
| JTBD analysis, 5 Whys | **Problem statement** | Summary + Description | `summary` + `description` | Summary: plain text (one line). Description: Markdown |
| User Journey Analysis | **User flow** | Description (flow section) | `description` | Markdown (numbered steps) |
| MoSCoW Prioritization | **Priority signal** | Priority | `priority` | `{ "id": "<id>" }` — see catalog |
| JTBD + User Journey | **Acceptance criteria** | Acceptance Criteria | `customfield_10036` | Markdown (Given/When/Then list) |
| User Journey (edge cases) | **Test criteria** | Test Steps | `customfield_10062` | Markdown (numbered steps) |
| — | **UI specification** | Description (design section) | `description` | Markdown (Figma link + notes) |
| — | **API / Data contract** | Description (API section) | `description` | Markdown (endpoint table) |
| — | **File impact** | Description (architecture section) | `description` | Markdown (file list) |
| — | **Dependencies** | Linked Issues + Description | `issuelinks` + `description` | Links via API; list in description |
| — | **Non-functional requirements** | Description (NFR section) | `description` | Markdown (bullet list) |
| — | **Out of scope** | Description (exclusions section) | `description` | Markdown (bullet list) |

### Mapping Rules

- **Summary** (`summary`): One-line problem statement. Max ~120 characters. No markdown.
- **Description** (`description`): All detailed sections go here, structured with Jira headings (`h2.` format when using ADF, `##` when using Markdown via MCP).
- **Acceptance Criteria** (`customfield_10036`): Only verifiable Given/When/Then criteria. No narrative.
- **Test Steps** (`customfield_10062`): Numbered manual verification steps derived from acceptance criteria and edge cases.
- **Priority** (`priority`): Map MoSCoW → Priority: Must Have → High/Highest, Should Have → Medium, Could Have → Low, Won't Have → do not set.

---

## 2. Field Catalog — System Fields

| Field | Key | Schema | Format (API / MCP) | Catalog |
|---|---|---|---|---|
| Summary | `summary` | string | Plain text (single line) | — |
| Description | `description` | string | Markdown via MCP `editJiraIssue` (converted to ADF) | — |
| Assignee | `assignee` | user | `{ "accountId": "<id>" }` | Dynamic (user picker) |
| Priority | `priority` | priority | `{ "id": "<id>" }` | See below |
| Labels | `labels` | array of string | `["label1", "label2"]` | Free text |
| Components | `components` | array of component | `[{ "id": "<id>" }]` | See below |
| Fix Versions | `fixVersions` | array of version | `[{ "id": "<id>" }]` | Dynamic (project versions) |
| Linked Issues | `issuelinks` | array of issuelink | Via issue link API | — |
| Issue Type | `issuetype` | issuetype | `{ "id": "10005" }` (Story) | — |
| Parent (Epic) | `parent` | issuelink | `{ "key": "MAX-XXXXX" }` | — |
| Project | `project` | project | `{ "key": "MAX" }` or `{ "id": "10022" }` | — |
| Attachment | `attachment` | array of attachment | Via attachment API (not edit body) | — |

### Priority Catalog

| ID | Name | When to use |
|---|---|---|
| `1` | Highest | Critical blocker, production down |
| `2` | High | Must Have (MoSCoW), significant user impact |
| `3` | Medium | Should Have (MoSCoW), moderate impact |
| `10000` | Needs Priority | Default — not yet triaged |
| `4` | Low | Could Have (MoSCoW), minor impact |
| `5` | Lowest | Nice to have, no urgency |

### Components Catalog

| ID | Name | When to use |
|---|---|---|
| `10047` | maX Android | Android-specific work |
| `10048` | maX BE | Backend-specific work |
| `10046` | maX iOS | iOS-specific work |
| `10045` | maX Web | Web-specific work |
| `10036` | Technical Debt | Refactoring, tech debt reduction |

---

## 3. Field Catalog — Custom Fields

| Field | Key | Schema | Format (API / MCP) | Catalog |
|---|---|---|---|---|
| Team | `customfield_10001` | team | Team ID | Dynamic (team picker) |
| Sprint | `customfield_10020` | array (json) | Sprint ID | Dynamic (board sprints) |
| Target Start | `customfield_10022` | date | `YYYY-MM-DD` | — |
| Target End | `customfield_10023` | date | `YYYY-MM-DD` | — |
| Story Points | `customfield_10028` | number (float) | Plain number | — |
| Acceptance Criteria | `customfield_10036` | string (textarea) | Markdown (prefer) or ADF | — |
| Test Steps | `customfield_10062` | string (textarea) | Markdown (prefer) or ADF | — |
| QA Assignee | `customfield_10293` | user (userpicker) | `{ "accountId": "<id>" }` | Dynamic (user picker) |
| Blocker Comment | `customfield_10446` | string (textarea) | Markdown (prefer) or ADF | — |
| Dev Estimate | `customfield_10552` | option (select) | `{ "id": "<id>" }` | See below |
| Demo Date | `customfield_10553` | date | `YYYY-MM-DD` | — |

### Dev Estimate Catalog

| ID | Value | Rough guidance |
|---|---|---|
| `10931` | 4 hours | Small fix, single file, well-understood |
| `10932` | 1-2 Days | Standard feature increment, few files |
| `10933` | 3-5 days | Multi-file feature, integration work |
| `11081` | 10 Days | Large feature, multiple increments, cross-module |

---

## 4. Format Rules for MCP Writes

When using `atlassian-mcp` to write to Jira:

- **`editJiraIssue`** accepts Markdown for `description` — the MCP converts it to ADF.
- **Custom textarea fields** (`customfield_10036`, `customfield_10062`, `customfield_10446`): prefer Markdown string. If the instance rejects it, fall back to ADF.
- **Select/option fields** (`priority`, `customfield_10552`): MUST use `{ "id": "<id>" }` with a value from the catalog. Free text is rejected by the API.
- **Component fields**: MUST use `[{ "id": "<id>" }]` with IDs from the catalog. Names are not accepted.
- **User fields** (`assignee`, `customfield_10293`): require `accountId`. Do NOT set these unless the user explicitly provides the assignee.
- **Date fields**: ISO format `YYYY-MM-DD`. Do NOT set target dates unless explicitly provided.

### Content Structure in Description

When writing to `description`, use this heading structure for consistency:

```markdown
## Problem Statement
<problem framing>

## User Flow
1. <step>
2. <step>

## UI Specification
<Figma link and design notes>

## API / Data Contract
| Endpoint | Method | Request | Response |
|---|---|---|---|
| ... | ... | ... | ... |

## Architecture / File Impact
- <file>: <what changes>

## Dependencies
- <dependency>

## Non-Functional Requirements
- <requirement>

## Out of Scope
- <exclusion>
```

---

## 5. /enrich Push Behavior

When `/enrich --push` writes to Jira:

1. **Do NOT overwrite** the original description. Append after it with `h2. [Enhanced by Agent]` as separator.
2. **Acceptance Criteria** → write to `customfield_10036` (append, do not overwrite if content exists).
3. **Test Steps** → write to `customfield_10062` (append, do not overwrite if content exists).
4. **Priority** → set via `priority` field only if the current value is `Needs Priority` (`10000`). Do not override a human-set priority.
5. **Components** → set if the ticket has none and the platform is identifiable. Do not remove existing components.
6. **Dev Estimate** → set only if explicitly derived from `/charter` or user input. Do not guess.
7. All other fields (Sprint, Assignee, QA Assignee, dates) → do NOT set automatically. These are team decisions.
