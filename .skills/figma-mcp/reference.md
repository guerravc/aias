# Figma MCP — Tool Reference

Complete parameter reference for all tools and resources available in the Figma MCP server.

---

## Authentication

### whoami

| Field | Details |
|-------|---------|
| **Purpose** | Get information about the authenticated Figma user |
| **Parameters** | None |
| **When to use** | When experiencing permission issues; verify the correct user is authenticated |

---

## Design and Context

### get_design_context

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `nodeId` | string | Yes | Node ID in the Figma document (e.g. `"123:456"` or `"123-456"`) |
| `fileKey` | string | Yes | Figma file key (extracted from URL) |
| `clientLanguages` | string | No | Comma-separated languages (e.g. `"swift"`, `"kotlin"`, `"html,css,typescript"`). Use `"unknown"` if not known. |
| `clientFrameworks` | string | No | Comma-separated frameworks (e.g. `"swiftui"`, `"jetpack-compose"`, `"react"`). Use `"unknown"` if not known. |
| `forceCode` | boolean | No | Force code return even for large outputs. Only set when user explicitly requests it. |
| `disableCodeConnect` | boolean | No | Disable Code Connect. Only set when user explicitly requests it. |

**Returns:** Code string + JSON of download URLs for referenced assets.

**URL extraction:**
- Standard: `https://figma.com/design/:fileKey/:fileName?node-id=:int1-:int2`
  - `fileKey` = segment after `/design/`
  - `nodeId` = `node-id` param with `-` replaced by `:` (e.g. `node-id=1-2` → `"1:2"`)
- Branch: `https://figma.com/design/:fileKey/branch/:branchKey/:fileName`
  - Use `branchKey` as `fileKey`

### get_screenshot

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `nodeId` | string | Yes | Node ID in the Figma document |
| `fileKey` | string | Yes | Figma file key |
| `clientLanguages` | string | No | Same as `get_design_context` |
| `clientFrameworks` | string | No | Same as `get_design_context` |

**Returns:** Screenshot image of the specified node.

### get_metadata

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `nodeId` | string | Yes | Node ID in the Figma document |
| `fileKey` | string | Yes | Figma file key |
| `clientLanguages` | string | No | Same as `get_design_context` |
| `clientFrameworks` | string | No | Same as `get_design_context` |

**Returns:** Metadata about the file or node (properties, structure, information).

### get_figjam

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `nodeId` | string | Yes | Node ID in the FigJam document |
| `fileKey` | string | Yes | Figma file key |
| `clientLanguages` | string | No | Same as `get_design_context` |
| `clientFrameworks` | string | No | Same as `get_design_context` |

**Returns:** FigJam board content (whiteboards, brainstorms, diagrams).

---

## Variables and Tokens

### get_variable_defs

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `nodeId` | string | Yes | Node ID in the Figma document |
| `fileKey` | string | Yes | Figma file key |
| `clientLanguages` | string | No | Same as `get_design_context` |
| `clientFrameworks` | string | No | Same as `get_design_context` |

**Returns:** Design token and variable definitions.

---

## Code Connect

### get_code_connect_map

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `nodeId` | string | Yes | Node ID |
| `fileKey` | string | Yes | File key |
| `clientLanguages` | string | No | Same as `get_design_context` |
| `clientFrameworks` | string | No | Same as `get_design_context` |

**Returns:** Existing Code Connect mappings for the node.

### add_code_connect_map

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `nodeId` | string | Yes | Node ID |
| `fileKey` | string | Yes | File key |
| `clientLanguages` | string | No | Same as `get_design_context` |
| `clientFrameworks` | string | No | Same as `get_design_context` |

> **Write operation** — only call when the user explicitly requests it.

---

## Generation and Rules

### generate_diagram

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| Parameters similar to `get_design_context` | — | — | — |

> **Write operation** — only call when the user explicitly requests it.

### create_design_system_rules

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| Parameters similar to `get_design_context` | — | — | — |

> **Write operation** — only call when the user explicitly requests it.

---

## Available Documentation Resources

The Figma MCP exposes read-only documentation resources. Fetch these when you need implementation guidance for Code Connect or Figma integrations.

| Resource | Description |
|----------|-------------|
| `quickstart-guide` | Getting started with Figma MCP |
| `intro` | Introduction to Code Connect |
| `config-file` | Configuration file reference |
| `react` | React integration guide |
| `swiftui` | SwiftUI integration guide |
| `compose` | Jetpack Compose integration guide |
| `html` | HTML/CSS integration guide |
| `code-connect-ui-setup` | Code Connect UI setup |
| `code-connect-ui-github` | Code Connect GitHub integration |
| `templates` | Template reference (v1) |
| `templatesv2` | Template reference (v2) |
| `custom` | Custom parser reference |
| `no-parser` | Usage without a parser |
| `storybook` | Storybook integration |
| `ci-cd` | CI/CD integration guide |
| `github-permissions` | GitHub permissions reference |
| `common-issues` | Troubleshooting common issues |
| `comparing-cc` | Comparing Code Connect approaches |

---

## Typical Call Flow

```
1. Extract fileKey and nodeId from Figma URL
2. get_design_context(fileKey, nodeId, clientLanguages, clientFrameworks)  → code + assets
   OR
   get_screenshot(fileKey, nodeId)  → visual reference
   OR
   get_metadata(fileKey, nodeId)  → node properties
3. If permission error → whoami() to verify authenticated user
```
