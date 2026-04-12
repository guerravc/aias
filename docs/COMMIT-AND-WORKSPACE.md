# /commit and Repository Resolution (Agnostic)

## Objective

Enable the `/commit` command to resolve the git repository from the **current workspace** (.code-workspace) in a platform- and technology-agnostic way.

## Repository Resolution (Order)

1. **Workspace (preferred)**  
   - If the current context is a workspace with `folders` and at least one folder is the root of a git repository, use that path(s) as **commit root(s)**.  
   - Single folder: that path becomes the `PROJECT_ROOT` for git.  
   - Multiple folders: default to the **first folder** that is a git root; the workspace MAY indicate which folder to use via a setting (see below).

2. **Fallback: projects.json**  
   - If there is no workspace, or no workspace folder is a git root, or the user invokes `/commit <projectAlias>`, use `${HOME}/.cursor/projects.json`: resolve `basePath` + `projects.<alias>.repoDir` (or `defaults.project`) to obtain `PROJECT_ROOT`.

## .code-workspace Convention

Optional, for multi-root workspaces where you need to specify **which folder** to commit in:

- **cursor.commitRoot** (string or number):  
  - `"<folderName>"`: name of the folder in `folders[].name` that is the repository to commit in.  
  - `0`: use the first folder (by index).  
  - If not defined and there is only one folder, that one is used.

Example (single repo — `cursor.commitRoot` not needed):

```json
{
  "folders": [
    { "path": "/path/to/my-app", "name": "my-app" }
  ],
  "settings": {
    "cursor.projectType": "app"
  }
}
```

Example (multi-root: config + app; commit only in app):

```json
{
  "folders": [
    { "path": "/path/to/aias-workspace", "name": "aias-workspace" },
    { "path": "/path/to/my-app", "name": "my-app" }
  ],
  "settings": {
    "cursor.commitRoot": "my-app"
  }
}
```

## Summary

- **Agnostic commit:** resolution is **workspace-first** (.code-workspace folders that are git roots), with **fallback** to projects.json.  
- **Workspace:** optionally use `cursor.commitRoot` to select a folder in multi-root setups; `cursor.projectType` MAY still be used for hints without `/commit` requiring it.  
- **projects.json:** optional project catalog for tooling scripts; not required for `/commit` when using a workspace.
