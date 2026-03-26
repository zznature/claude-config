---
description: Create a git commit following project conventions
---

# Git Commits Workflow

## Commit Message Format

```
<type>(<scope>): <description>

<body>
```

### Header

- `type`: feat | fix | refactor | docs | test | chore | perf | ci
- `scope`: optional, module name (e.g., eval, agents, benchmarks)
- `description`: imperative mood, lowercase, no period, max 72 chars

### Body — Answer Four Questions

Write a body when the change is non-trivial. Each line wraps at 72 chars.

1. **What problem forced this change?** — The trigger, not the solution
2. **What alternatives were considered?** — Shows the decision was deliberate
3. **What are the trade-offs?** — Performance, complexity, maintainability
4. **What might be surprising?** — Non-obvious side effects or constraints

Not every commit needs all four — include what is relevant.

### Examples

```
fix(validation): check formula before FG consistency

Formula mismatch was caught by the slower FG check, causing
unnecessary SMARTS matching. Reordering lets the cheap check
run first and short-circuit.

Alternative: run both in parallel. Rejected because FG check
depends on formula filtering results.
```

```
feat(eval): add stratified sampling with checkpoint resume

Problem: full eval takes 3h. Need quick sanity checks during dev.

Trade-off: stratified sampling guarantees at least 1 item per
sub_category, but small categories may be over-represented.
```

```
refactor: simplify state passing between agents
```

### Prohibited

- Do NOT include any AI attribution footers or signatures
- Do NOT use `Co-Authored-By` lines referencing AI tools
