# Model Routing for Token Efficiency

## Principle

Route subtasks to the cost-effective models. Sonnet acts as orchestrator and delegates via the Agent tool's `model` parameter.

## Model Tiers

**Haiku** — lightweight, fast, high-frequency tasks (~90% of Sonnet capability, 3x cheaper)
- File reading, grep/glob search, doc lookup
- Summarizing existing content with no synthesis required
- Worker agents invoked repeatedly in a loop

**Sonnet** — best coding model, default orchestrator
- All code writing, editing, testing, debugging, refactoring
- Orchestrating multi-agent workflows
- Complex coding tasks and most implementation work

**Opus** — deepest reasoning, highest cost
- Complex architectural decisions and trade-off analysis
- Ambiguous goals requiring first-principles reasoning
- Research, planning, and tasks where quality matters

## Routing Rules

- **Delegate to Opus** when a task requires deep reasoning *before* any code is written. Spawn via Agent tool with `model="opus"`, pass full context, then act on its output in Sonnet.
- **Delegate to Haiku** for pure retrieval with no reasoning. Use `subagent_type="Explore"` or Agent with `model="haiku"`.
- **Stay in Sonnet** for all implementation. Do not delegate code writing or editing.
- **Never over-delegate**: if the task fits in one response, handle it inline. Delegation has overhead — only delegate substantial subtasks.
- **Compose freely**: a single request may fan out to Opus (plan) → Sonnet (implement). That is the intended pattern.

## Example Triggers

- "How should we design the resource lock manager?" → delegate to `opus`
- "Read all test files and summarize coverage gaps" → delegate to `haiku` (Explore)
- "Add a new parameter to the Keithley driver" → handle inline with `sonnet`
- "Refactor guard.py and explain the trade-offs" → Opus for trade-offs, Sonnet for the edit
