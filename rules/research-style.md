# Research Project Style

## Principles

- This is a **research project** — prioritize agile, iterative development over production-grade engineering
- Favor rapid prototyping: get a working version first, refine later
- Embrace cutting-edge ideas, models, and techniques
- Keep code modular enough to swap components easily when better methods emerge

## Guidelines

**Iterate fast, validate early** — ship a minimal experiment, measure results, then decide whether to invest more. Avoid premature abstraction.

**Keep code research-friendly** — flat module structure, explicit state, minimal indirection. Aimed to publish as open source.

**Stay reproducible** — pin dependencies, log random seeds, checkpoint intermediate results. Every eval run should be re-creatable from its config.
