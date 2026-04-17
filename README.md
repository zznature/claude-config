# Claude Code Configs for Research Projects

Reusable Claude Code settings, rules, agents, commands, and skills.
Designed to be shared across multiple research projects via git submodule.

## Directory Structure

```
.claude/
├── CLAUDE.md                  # User-level preferences (extends project root CLAUDE.md)
├── settings.json              # Permissions: allowed/denied tools (committed, shared)
├── settings.local.json        # Local-only permissions (gitignored, per-machine)
│
├── rules/                     # Always-active behavioral rules
│   ├── first-principles.md    #   Challenge assumptions, reason from fundamentals
│   ├── research-style.md      #   Iterate fast, stay reproducible, skip ceremony
│   ├── write-for-future-readers.md  # Capture why, not what
│   ├── explanatory-mode.md    #   Insert ★ Insight blocks for non-obvious decisions
│   └── model-routing.md       #   Route subtasks to Haiku/Sonnet/Opus by difficulty
│
├── commands/                  # User-invokable slash commands
│   ├── python-review.md       #   /python-review — lint + type check + security scan
│   ├── git-commits.md         #   /git-commits — conventional commit workflow
│   └── codemap-updater.md     #   /codemap-updater — refresh architecture docs
│
├── agents/                    # Autonomous agents (invoked by commands or directly)
│   ├── python-reviewer.md     #   Code review: CRITICAL/HIGH/MEDIUM severity
│   ├── codemap-updater.md     #   Scan codebase, generate token-lean architecture docs
│   └── doc-updater.md         #   Detect doc drift, fix discrepancies
│
└── skills/                    # On-demand knowledge (loaded when relevant)
    ├── python-patterns/       #   Type hints, error handling, idioms, anti-patterns
    ├── python-testing/        #   pytest fixtures, parametrize, mocking LLM calls
    ├── writing-for-engineers/ #   Comments, commit messages, READMEs — capture the why
    ├── agent-harness-construction/  # Action space design, observation formatting
    └── explanatory-output/    #   Explanatory insight block generation
```

## What Claude Code reads automatically

| Path | When loaded | Purpose |
|------|-------------|---------|
| `CLAUDE.md` | Every conversation | User preferences, code style, philosophy |
| `rules/*.md` | Every conversation | Behavioral constraints |
| `settings.json` | On startup | Tool permissions (shared) |
| `settings.local.json` | On startup | Tool permissions (local, gitignored) |
| `commands/*.md` | When user types `/command` | Slash command definitions |
| `agents/*.md` | When agent is invoked | Agent system prompts |
| `skills/*/SKILL.md` | When skill is activated | Domain knowledge |

`settings.json` and `settings.local.json` are merged on startup.
This `README.md` is not consumed by Claude Code.

## Adding to a new project

```bash
# As a git submodule
git submodule add <repo-url> .claude

# Or copy directly
cp -r /path/to/shared/.claude/ .claude/
```

Then create a project-level `CLAUDE.md` at the repo root with project-specific
architecture, CLI commands, and environment setup. The `.claude/CLAUDE.md`
extends it with your personal preferences.

## Customizing per project

- **`settings.local.json`** — Add project-specific permissions (e.g. conda env name).
  This file is gitignored and stays local.
- **Project root `CLAUDE.md`** — Project architecture, repo structure, key commands.
  This is the primary context file Claude Code reads.

## Design principles

- **Generic over specific** — No hardcoded package names, paths, or API keys
- **Thin commands, rich agents** — Commands are launchers; agents have the logic
- **Token-lean** — Files should be optimized for AI context consumption
