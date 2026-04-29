# User Preferences

User-level settings that extend (not override) the project root `CLAUDE.md`.
This file lives in `.claude/` and may be committed per user preference.

## Code Style

- No emojis in code, comments, or documentation
- Prefer immutability — avoid mutating objects or arrays in place
- Many small files over few large files; 200-400 lines typical, 800 max per file
- No backwards-compatibility shims for code that has no known callers

## Testing

- Test-Driven Development (TDD): write tests before implementation when the interface is clear
- 80% minimum coverage; unit + integration + E2E for critical flows
- Keep tests fast and deterministic — avoid real network/filesystem calls in unit tests

## Knowledge Capture

- Personal debugging notes and temporary context -> auto memory (`~/.claude/projects/*/memory/`)
- Team/project knowledge (architecture decisions, API changes, runbooks) -> project docs
- Do not duplicate knowledge that is already captured in nearby docs or comments

## Privacy & Security

- Never paste secrets, API keys, tokens, passwords, or JWTs into any output or file
- Review content for sensitive data before suggesting it be shared externally

---

**Philosophy**: Parallel execution, plan before action, test before code, security always.
