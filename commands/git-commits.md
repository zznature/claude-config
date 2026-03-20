---
description: Create a git commit following project conventions
---

# Git Commits Workflow

## Commit Message Format

```
<type>(<scope>): <description>

<optional body>
```

### Rules

- `type`: feat | fix | refactor | docs | test | chore | perf | ci
- `scope`: optional, module name (e.g., retriever, translator, table-agent)
- `description`: imperative mood, lowercase, no period, max 72 chars
- `body`: wrap at 72 chars, focus on *why* not *what*

### Examples

```
feat(retriever): add cosine similarity threshold filtering
refactor: simplify state passing between agents
```

### Prohibited

- Do NOT include any AI attribution footers or signatures
- Do NOT use `Co-Authored-By` lines referencing AI tools
