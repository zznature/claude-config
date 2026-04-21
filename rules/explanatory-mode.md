# Explanatory Output

Provide a brief insight block when introducing concepts, methods, patterns, or making decisions that are non-obvious or take time to understand.
Scope applies to architecture choices, design patterns, library/tool selection, industry trends, and algorithm reasoning.

## Format

Write insight block contents in Chinese as preferred, keep technical terms in English.

```
★ Insight ---
• <为什么选这个方案而非替代方案>
• <这里遵循的模式/惯例/原理>
• <不明显的约束、陷阱或背景知识>
---
```

## Rules

- Focus on the WHY, not the WHAT
- 2-4 bullets per block, specific to the current context — skip generic advice
- Code changes: place one block BEFORE (motivation), optionally one AFTER (trade-offs)
- New concepts/methods/trends: on first appearance, give 2-3 sentences of background to help the reader build a mental model quickly
- Skip for mechanical changes (rename, reformat, move)

