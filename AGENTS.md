# Agent Guidelines

## Skills

The following skills are available in `skills/`. Read the relevant `SKILL.md` before beginning the task it covers.

| Skill                                                                                           | When to use                                                                                                             |
| ----------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| [`skills/clarify`](./skills/clarify/SKILL.md)                                                   | Before acting on any prompt — evaluate vagueness and ask at most 3 clarifying questions if needed                       |
| [`skills/planning`](./skills/planning/SKILL.md)                                                 | Stage 1: high-level planning → writes `docs/PLANNING.md`                                                                |
| [`skills/spec`](./skills/spec/SKILL.md)                                                         | Stage 2: technical specification → writes `docs/SPECIFICATION.md`                                                       |
| [`skills/design`](./skills/design/SKILL.md)                                                     | Stage 3: system design decisions → writes `docs/DESIGN.md`                                                              |
| [`skills/aggressive-refactoring`](./skills/aggressive-refactoring/SKILL.md)                     | Aggressively refactor code to remove repetition, modernize APIs, simplify interfaces, and delete stale structure safely |
| [`skills/react-performance-best-practices`](./skills/react-performance-best-practices/SKILL.md) | Framework-neutral React performance guidance for components, hooks, data fetching, bundle size, and rendering behavior  |
| [`skills/modern-java`](./skills/modern-java/SKILL.md)                                           | Modern stable Java and JUnit guidance for Java code, APIs, refactoring, reviews, and JVM tests                          |
| [`skills/tdd-workflow`](./skills/tdd-workflow/SKILL.md)                                         | Implementing features or fixes using strict red-green-refactor TDD                                                      |
| [`skills/typescript-quality-gates`](./skills/typescript-quality-gates/SKILL.md)                 | Running TypeScript lint, format, and type-check gates before finishing                                                  |
| [`skills/python-quality-gates`](./skills/python-quality-gates/SKILL.md)                         | Running Python `ty`, `ruff`, and test gates before finishing                                                            |
| [`skills/markdown-quality-gates`](./skills/markdown-quality-gates/SKILL.md)                     | Running `pnpm run format` from the repo root after any Markdown file changes                                            |

## Default Behavior

Always apply the `clarify` skill at the start of anew task.
