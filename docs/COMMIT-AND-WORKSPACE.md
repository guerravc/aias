# /commit y resolución de repositorio (agnóstico)

## Objetivo

Hacer que el comando `/commit` resuelva el repositorio git desde el **workspace actual** (.code-workspace) de forma agnóstica a la plataforma o tecnología.

## Resolución de repositorio (orden)

1. **Workspace (preferido)**  
   - Si el contexto actual es un workspace con `folders` y al menos una carpeta es la raíz de un repositorio git, usar esa(s) ruta(s) como **commit root(s)**.  
   - Con un solo folder: ese path es el `PROJECT_ROOT` para git.  
   - Con varios folders: por defecto usar el **primer folder** que sea git root; opcionalmente el workspace puede indicar cuál usar con una setting (ver más abajo).

2. **Fallback: projects.json**  
   - Si no hay workspace, o ninguna carpeta del workspace es un git root, o el usuario invoca `/commit <projectAlias>`, usar `${HOME}/.cursor/projects.json`: resolver `basePath` + `projects.<alias>.repoDir` (o el `defaults.project`) para obtener `PROJECT_ROOT`.

## Convención en el .code-workspace

Opcional, para workspaces multi-root donde hace falta indicar **en qué carpeta** hacer commit:

- **cursor.commitRoot** (string o number):  
  - `"<folderName>"`: nombre del folder en `folders[].name` que es el repo donde hacer commit.  
  - `0`: usar el primer folder (por índice).  
  - Si no está definido y hay una sola carpeta, se usa esa.

Ejemplo (un solo repo, no hace falta `cursor.commitRoot`):

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

Ejemplo (multi-root: config + app; commit solo en el app):

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

## Resumen

- **Commit agnóstico:** resolución **workspace-first** (carpetas del .code-workspace que sean git roots), **fallback** a projects.json.  
- **Workspace:** opcionalmente `cursor.commitRoot` para elegir carpeta en multi-root; `cursor.projectType` puede seguir usándose para hints sin que commit lo use obligatoriamente.  
- **projects.json:** catálogo opcional de proyectos para tooling scripts; no requerido para `/commit` si se usa un workspace.
