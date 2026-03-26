# Write for Future Readers

## Principle

Every comment, commit message, and README is written for someone who lacks your current context — a teammate joining later, a maintainer inheriting the code, or yourself in six months.

## Rules

- Capture the **why**, not the **what**. The code shows what; comments explain why.
- Never write comments that merely narrate the code (`// increment counter`). If the code is unclear, rewrite the code.
- Every magic number, load-bearing implementation choice, and deliberate non-obvious decision must have a comment explaining the reasoning.
- Commit message bodies answer: what problem forced this change, what alternatives were considered, and what trade-offs remain.
- TODOs must include enough context to be actionable without the author present.
