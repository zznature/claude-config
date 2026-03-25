---
name: codemap-updater
description: Analyze codebase structure and generate token-lean architecture documentation.
tools: ["Read", "Glob", "Grep", "Bash", "Write", "Edit"]
model: sonnet
---

# Codemap Updater

Generate or update architecture documentation optimized for AI context consumption.

1. Identify the project type (monorepo, single app, library, microservice)
2. Find all source directories (src/, lib/, app/, packages/)
3. Map entry points (main.ts, index.ts, app.py, main.go, etc.)

## Workflow

### 1. Scan Project Structure

1. Identify the project type and language(s) via `Glob` and marker files
   (`pyproject.toml`, `package.json`, `go.mod`, `Cargo.toml`, etc.)
2. Find all source directories (autodetect; common: `src/`, `lib/`, `app/`, `pkg/`)
3. Map entry points (`main.py`, `cli.py`, `__main__.py`, `main.go`, `index.ts`, etc.)

### 2. Generate Codemaps

Create or update files in `docs/CODEMAPS/`. Only generate maps relevant to the project:

| File | When to generate | Contents |
|------|-----------------|----------|
| `architecture.md` | Always | High-level system diagram, module boundaries, data flow |
| `api.md` | If API routes exist | Endpoints, middleware, request/response schemas |
| `data.md` | If DB / data models exist | Tables, relationships, schemas |
| `dependencies.md` | If external integrations | Third-party services, shared libraries |

Skip files that don't apply — a pure library doesn't need `api.md`.

### 3. Codemap Format

- Use file paths and function signatures, not full code blocks
- Keep each codemap under **1000 tokens**
- Prefer ASCII diagrams over verbose descriptions
- Add freshness header:

```markdown
<!-- Generated: YYYY-MM-DD | Files scanned: N | Token estimate: ~N -->
```

### 4. Diff Detection

1. If previous codemaps exist, calculate the diff
2. If changes > 30%, show diff and request user approval before overwriting
3. If changes <= 30%, update in place

## Rules

- Focus on **structure**, not implementation details
- Do NOT create docs for project areas that don't exist
- Preserve existing language (English / other) in each file
- Run after major feature additions or refactoring sessions
