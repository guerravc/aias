# Xcode Tools MCP — Tool Reference

Complete parameter reference for all tools available in the Xcode Tools MCP server.

---

## Windows and Context

### XcodeListWindows

| Field | Details |
|-------|---------|
| **Purpose** | List current Xcode windows and their workspace information |
| **Parameters** | None |
| **Returns** | Window descriptions including `tabIdentifier` for each workspace |

---

## Build

### BuildProject

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |

**Returns:** Build result (success/failure), elapsed time, list of errors with file path and line details.

### GetBuildLog

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `severity` | string | No | Filter by severity: `"error"` \| `"warning"` \| `"remark"` (default: `"error"`) |
| `pattern` | string | No | Regex filter — matches issue `message` field, task descriptions, command lines, console output |
| `glob` | string | No | Glob filter — matches issue `path` field and build task locations |

**Returns:** Build log entries (limited to 100 issues). Also indicates if build is in progress.

---

## Testing

### RunAllTests

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |

**Returns:** Test results (limited to 100, failures first). Includes path to full summary text file.

### RunSomeTests

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `tests` | array | Yes | Array of test specifiers (see below) |

**Test specifier object:**

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `targetName` | string | Yes | Test target name |
| `testIdentifier` | string | Yes | Test identifier in XCTestIdentifier format |

**Returns:** Test results (limited to 100, failures first).

### GetTestList

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |

**Returns:** Up to 100 tests. Full list written to `fullTestListPath` in grep-compatible format.

**Search keys for grep:** `TEST_TARGET`, `TEST_IDENTIFIER`, `TEST_FILE_PATH`.

---

## Previews and Snippets

### RenderPreview

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `sourceFilePath` | string | Yes | Xcode project path to the file (e.g. `ProjectName/Sources/MyView.swift`) |
| `previewDefinitionIndexInFile` | integer | No | Zero-based index of `#Preview` or `PreviewProvider` in file (default: 0) |
| `timeout` | integer | No | Seconds to wait for render (default: 120) |

**Returns:** Path to the rendered preview snapshot image.

### ExecuteSnippet

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `codeSnippet` | string | Yes | Swift code to execute |
| `sourceFilePath` | string | Yes | Xcode project path of the context file (snippet can access `fileprivate` declarations) |
| `timeout` | integer | No | Seconds to wait for execution (default: 120) |

**Returns:** Console output from `print` statements in the snippet.

> **Note:** Only available for source files in targets that compile apps, frameworks, libraries, or command-line executables.

---

## Documentation

### DocumentationSearch

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `query` | string | Yes | Search query (semantic matching) |
| `frameworks` | array | No | Framework names to limit search (searches all if omitted) |

**Returns:** Relevant documents with title, URI, content, and match score.

> **Note:** Does NOT require `tabIdentifier`.

---

## Diagnostics

### XcodeRefreshCodeIssuesInFile

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `filePath` | string | Yes | Xcode project path to the file |

**Returns:** Formatted diagnostics with severity levels and messages, diagnostics count.

### XcodeListNavigatorIssues

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `severity` | string | No | Filter: `"error"` \| `"warning"` \| `"remark"` (default: `"error"`) |
| `pattern` | string | No | Regex filter — matches issue `message` field |
| `glob` | string | No | Glob filter — matches issue `path` field |

**Returns:** Issues from Xcode's Issue Navigator (limited to 100). Includes build issues, package resolution problems, workspace config issues.

---

## File Operations

### XcodeRead

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `filePath` | string | Yes | Xcode project path to the file |
| `offset` | integer | No | Line number to start reading (for large files) |
| `limit` | integer | No | Number of lines to read (for large files) |

**Returns:** File content with line numbers (up to 600 lines by default). File size, total lines, lines read, start line.

### XcodeWrite

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `filePath` | string | Yes | Xcode project path for the file |
| `content` | string | Yes | Content to write |

**Returns:** Bytes written, lines written, whether file already existed. Auto-adds new files to project structure.

> **Write operation** — only when user or active mode authorizes modifications.

### XcodeUpdate

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `filePath` | string | Yes | Xcode project path to the file |
| `oldString` | string | Yes | Text to replace (must exist in file) |
| `newString` | string | Yes | Replacement text (must differ from `oldString`) |
| `replaceAll` | boolean | No | Replace all occurrences (default: `false`) |

**Returns:** Number of replacements, content length before/after.

> **Write operation** — only when user or active mode authorizes modifications.

---

## File System Operations

### XcodeLS

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `path` | string | Yes | Project path to explore (e.g. `ProjectName/Sources/`) |
| `recursive` | boolean | No | List recursively (default: `true`, truncated to 100 lines) |
| `ignore` | array | No | Patterns to exclude from listing |

### XcodeGlob

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `pattern` | string | No | Wildcard pattern (e.g. `*.swift`, `**/*.json`). Default: `**/*` |
| `path` | string | No | Project directory to search (default: root) |

**Returns:** Matching files sorted by most recently modified (truncated to 100).

### XcodeGrep

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `pattern` | string | Yes | Regex search pattern (**required — tool fails without it**) |
| `path` | string | No | File or directory in project (default: root) |
| `glob` | string | No | File pattern filter |
| `type` | string | No | File type shortcut: `swift`, `js`, `py`, etc. |
| `outputMode` | string | No | `"content"` \| `"filesWithMatches"` \| `"count"` (default: `"filesWithMatches"`) |
| `ignoreCase` | boolean | No | Case-insensitive matching |
| `multiline` | boolean | No | Allow patterns to span lines |
| `showLineNumbers` | boolean | No | Show line numbers (content mode only) |
| `linesBefore` | integer | No | Context lines before match |
| `linesAfter` | integer | No | Context lines after match |
| `linesContext` | integer | No | Context lines before and after |
| `headLimit` | integer | No | Stop after N results |

### XcodeMakeDir

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `directoryPath` | string | Yes | Project-relative path for the new directory |

> **Write operation** — only when user or active mode authorizes modifications.

### XcodeMV

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `sourcePath` | string | Yes | Project-relative source path |
| `destinationPath` | string | Yes | Project-relative destination path or new name |
| `operation` | string | No | `"move"` \| `"copy"` |
| `overwriteExisting` | boolean | No | Overwrite files at destination |

> **Write operation** — only when user or active mode authorizes modifications.

### XcodeRM

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `tabIdentifier` | string | Yes | Workspace tab identifier |
| `path` | string | Yes | Project path to delete |
| `deleteFiles` | boolean | No | Also trash underlying files (default: `true`) |
| `recursive` | boolean | No | Delete directories and contents recursively |

> **Write operation** — only when user or active mode authorizes modifications.

---

## Typical Call Flows

### Build and check errors
```
1. XcodeListWindows                     → tabIdentifier
2. BuildProject(tabIdentifier)          → success/failure
3. GetBuildLog(tabIdentifier)           → detailed errors (if build failed)
```

### Run specific tests
```
1. XcodeListWindows                     → tabIdentifier
2. GetTestList(tabIdentifier)           → available tests + fullTestListPath
3. (grep fullTestListPath for target/identifier)
4. RunSomeTests(tabIdentifier, tests)   → test results
```

### Preview a SwiftUI view
```
1. XcodeListWindows                     → tabIdentifier
2. RenderPreview(tabIdentifier, sourceFilePath) → snapshot image path
```

### Edit a file and verify
```
1. XcodeListWindows                     → tabIdentifier
2. XcodeRead(tabIdentifier, filePath)   → current content
3. XcodeUpdate(tabIdentifier, filePath, oldString, newString) → replacement
4. XcodeRefreshCodeIssuesInFile(tabIdentifier, filePath) → diagnostics
```
