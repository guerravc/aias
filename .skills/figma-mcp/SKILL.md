---
name: figma-mcp
description: Get design context, screenshots, and metadata from Figma via the Figma MCP. Use when the user provides a Figma URL, mentions a Figma file or node, or asks to reference a design.
---

# Figma MCP

## PURPOSE

Teach the agent how to interact with the Figma MCP server to obtain design context, screenshots, metadata, and Code Connect maps using the correct parameter extraction and call sequences.

---

## PREREQUISITES

No preliminary authentication call is required. Figma MCP authenticates via the configured token. If permission issues arise, call `whoami` to verify the authenticated user.

---

## URL PARSING

Most Figma operations require `fileKey` and `nodeId`. Extract them from URLs:

**Standard URL format:**
```
https://figma.com/design/:fileKey/:fileName?node-id=:int1-:int2
```
- `fileKey`: the segment after `/design/` (e.g. `pqrs` from `.../design/pqrs/ExampleFile?...`)
- `nodeId`: from `node-id` query parameter, replace `-` with `:` (e.g. `node-id=1-2` → `nodeId = "1:2"`)

**Branch URL format:**
```
https://figma.com/design/:fileKey/branch/:branchKey/:fileName
```
- Use `branchKey` as `fileKey` (not the original `fileKey`).
- Extract `nodeId` from the query parameter as above.

If the user provides a `nodeId` directly (e.g. `"123:456"` or `"123-456"`), normalize it to the colon format (`"123:456"`).

---

## OPERATIONS

### Get Design Context (Code Generation)

**When:** User asks to generate UI code from a Figma design, or wants implementation details for a component.

**Call:**
`get_design_context(fileKey, nodeId)`

**Required parameters:**
- `fileKey` (string): file key extracted from URL
- `nodeId` (string): node ID in colon format (e.g. `"123:456"`)

**Optional parameters:**
- `clientLanguages` (string): comma-separated languages (e.g. `"swift"`, `"kotlin"`, `"html,css,typescript"`). Use `"unknown"` if not known.
- `clientFrameworks` (string): comma-separated frameworks (e.g. `"swiftui"`, `"jetpack-compose"`, `"react"`). Use `"unknown"` if not known.
- `forceCode` (boolean): force code return even for large outputs. Only set when the user explicitly requests it.
- `disableCodeConnect` (boolean): disable Code Connect. Only set when the user explicitly requests it.

**Response:** Contains a code string and a JSON of download URLs for referenced assets.

---

### Get Screenshot

**When:** User asks for a visual reference or screenshot of a Figma component or screen.

**Call:**
`get_screenshot(fileKey, nodeId)`

**Required parameters:**
- `fileKey` (string)
- `nodeId` (string)

**Optional parameters:**
- `clientLanguages`, `clientFrameworks` (same as above)

---

### Get Metadata

**When:** User asks about properties, structure, or information of a Figma file or node without needing code.

**Call:**
`get_metadata(fileKey, nodeId)`

**Parameters:** Same as `get_design_context`.

---

### Get Variable Definitions

**When:** User asks about design tokens, variables, or design system values.

**Call:**
`get_variable_defs(fileKey, nodeId)`

**Parameters:** Same as `get_design_context`.

---

### Code Connect

**Get map:**
`get_code_connect_map(fileKey, nodeId)` — retrieve existing Code Connect mappings.

**Add map:**
`add_code_connect_map(fileKey, nodeId)` — add new Code Connect mappings.

**Note:** `add_code_connect_map` is a write operation — only call when the user explicitly requests it.

---

### Get FigJam Content

**When:** User asks about FigJam boards (whiteboards, brainstorms, diagrams).

**Call:**
`get_figjam(fileKey, nodeId)`

**Parameters:** Same as `get_design_context`.

---

### Design System Rules

**When:** User asks to create or manage design system rules.

**Call:**
`create_design_system_rules(fileKey, nodeId)`

**Note:** This is a write operation — only call when the user explicitly requests it.

---

## AVAILABLE RESOURCES

The Figma MCP also exposes read-only documentation resources that can be fetched:
- `quickstart-guide`, `intro`, `config-file`
- `react`, `swiftui`, `compose`, `html`
- `code-connect-ui-setup`, `code-connect-ui-github`
- `templates`, `templatesv2`, `custom`, `no-parser`
- `storybook`, `ci-cd`, `github-permissions`
- `common-issues`, `comparing-cc`

Fetch these when you need implementation guidance for Code Connect or Figma integrations.

---

## REFERENCE

For complete parameter details, types, and return values for every tool, see [reference.md](reference.md).

---

## SAFETY RULES

**Read-only by default:**
- `get_design_context`, `get_screenshot`, `get_metadata`, `get_variable_defs`, `get_figjam`, `get_code_connect_map`, `whoami`: always allowed.
- `add_code_connect_map`, `create_design_system_rules`, `generate_diagram`: **only when the user explicitly asks**.

**Abort on failure:**
- If Figma returns a permission error: call `whoami` to verify the authenticated user, report the issue, and ask the user to check access.
- If the URL cannot be parsed (missing `fileKey` or `nodeId`): ask the user to provide a valid Figma URL or explicit IDs.
- If the MCP returns an error or timeout: abort and report the error.

**Data integrity:**
- Never invent design data, measurements, or colors that the API did not return.
- If a response is empty or truncated, report it as such.
