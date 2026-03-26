---
name: writing-for-engineers
description: Patterns for code comments, commit messages, and READMEs that capture hard-earned knowledge for future readers.
origin: custom
---

# Writing for Engineers

Guide for one-way communication in code: comments, commit messages, and READMEs.
The goal is always to convey the **why**, not the **what**.

## When to Activate

- Writing or reviewing code comments
- Drafting commit messages
- Creating or updating READMEs
- Reviewing documentation for completeness

---

## Code Comments

Most comments are noise. Good comments explain things the code itself cannot.

### Worth writing

| Type | Purpose | Example |
|------|---------|---------|
| **TODO** | Mark deferred work with actionable context | `TODO: O(n^2) loop is fine for n<100; needs indexing if we scale to full dataset` |
| **Reference** | Link to paper, spec, or source (use permalinks) | `# Implements Algorithm 3 from arxiv.org/abs/2301.xxxxx, adapted for batch input` |
| **Correctness argument** | Explain why non-trivial steps produce correct results | `# Sorting here guarantees deterministic hashing below` |
| **Hard-learned lesson** | Document a non-obvious fix that cost real debugging time | `# Must flush before close — otherwise last batch silently dropped (cost 2h to find)` |
| **Rationale for constants** | Explain where magic numbers come from | `# 2048 = max context tokens minus prompt overhead (measured empirically)` |
| **Load-bearing choice** | Flag when correctness depends on a seemingly innocent detail | `# Must be OrderedDict — iteration order matters for downstream serialization` |
| **"Why not"** | Explain why the obvious approach was avoided | `# Not using list comprehension here because we need to break on first error` |

### Not worth writing

```python
# bad — restates the code
i += 1  # increment i

# bad — no actionable context
# TODO: fix this

# bad — will rot immediately
# Returns the user object (what if it changes to return a dict?)
```

### Comment style

- Place the comment **before** the code it explains, not after
- If a comment is longer than 3 lines, consider whether it belongs in a docstring or a doc file instead
- Note any divergences from a referenced source

---

## Commit Messages

Commit messages are the historical record of **why** the codebase evolved.
The body should answer: what problem, what alternatives, what trade-offs, what surprises.

See `commands/git-commits.md` for format, examples, and the full four-question framework.

### Anti-patterns

- `fixed bug` — which bug? why did it happen?
- `updated code` — every commit updates code
- `WIP` — squash before merge, or add context

---

## READMEs

A README answers four questions, in this order:

1. **What does this do?** — One-liner + optional visual/demo
2. **Why should I care?** — The problem it solves
3. **How do I use it?** — Usage example (show before install)
4. **How do I install it?** — Setup steps

Structure like a funnel: someone should decide in 5 seconds whether this is relevant to them.
Show usage before installation — people want to see what they get before committing to setup.

### Template

```markdown
# Project Name

One-line description of what this does.

## Quick Example

\`\`\`python
result = my_tool("input")
print(result)  # expected output
\`\`\`

## Why

Brief explanation of the problem and why this solution exists.

## Installation

\`\`\`bash
pip install my-tool
\`\`\`

## Usage

More detailed examples, configuration, API reference.
```

---

## Quick Reference

| Writing | Capture the why | Avoid |
|---------|----------------|-------|
| Comment | Why this approach, not what the code does | Narrating code, empty TODOs |
| Commit message | What problem, what alternatives, what trade-offs | "fixed bug", "WIP", "updated" |
| README | What, why, usage, install (in that order) | Wall of text before showing usage |
