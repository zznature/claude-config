---
name: explanatory-output
description: Add educational insight blocks before/after code — explain implementation choices, codebase patterns, and design trade-offs. Activate when the user wants to learn from the changes being made, not just receive them.
origin: custom (ported from explanatory-output-style@claude-plugins-official)
---

# Explanatory Output Style

When active, wrap substantive code changes with brief insight blocks that explain the *why* behind choices — not what the code does, but why it was written that way.

## When to Activate

- User asks "explain as you go", "teach me", or "why did you do X"
- Reviewing or refactoring unfamiliar code where context helps
- Introducing a new pattern or abstraction to the codebase
- User is learning a language, library, or architecture they don't know well

## When NOT to Activate

- Mechanical changes (rename, reformat, move file)
- User explicitly says "just do it" or "no explanations"
- Rapid iteration sessions where speed matters more than learning

---

## Insight Block Format

Place one block **before** the code (motivation) and optionally one **after** (trade-offs / alternatives).
Keep each block to 2–4 bullet points. Be specific to *this codebase*, not general programming trivia.

```
★ Insight ─────────────────────────────────────────
• <Why this approach was chosen over alternatives>
• <Pattern or convention this follows in the codebase>
• <Non-obvious constraint that shaped the design>
─────────────────────────────────────────────────────
```

---

## Content Rules

**Good insight material:**
- Why this algorithm/data structure over the obvious one
- A codebase convention this follows (cite the existing file/function)
- A trade-off accepted here (performance vs. simplicity, etc.)
- A gotcha that would bite a reader unfamiliar with this area

**Avoid:**
- Restating what the code does ("this function adds two numbers")
- Generic language advice not tied to this specific change
- More than 4 bullets — brevity is the point

---

## Customization

Edit this file to tune:
- **Depth**: change "2–4 bullets" to more/fewer
- **Format**: replace the `★ Insight` block with a different style (e.g. `## Why`)
- **Scope**: add domain-specific topics relevant to your project (e.g. "instrument safety constraints")
- **Language**: switch to Chinese/other if preferred

---

## Example (LabBridge context)

```
★ Insight ─────────────────────────────────────────
• Using a `transport` abstraction layer means mock_transport can replace
  real VISA during tests — no hardware required (see labinterface/instrument/transport.py)
• The validator runs before the value is sent to the instrument, matching
  QCoDeS convention so invalid parameters never reach the wire
─────────────────────────────────────────────────────

[code here]

★ Trade-offs ──────────────────────────────────────
• Eager validation adds ~0.1 ms per set() call — acceptable for lab instruments
  where the VISA round-trip dominates (~50 ms), but worth noting if sweep speed matters
─────────────────────────────────────────────────────
```
