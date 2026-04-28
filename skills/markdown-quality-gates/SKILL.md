---
name: markdown-quality-gates
description: Enforce required verification for Markdown work. Use when Codex, Claude Code, or OpenCode adds or modifies any `.md` file. Before finishing, run `pnpm run format` from the top-level project directory so Prettier rewrites Markdown formatting. Do not skip or hide formatting failures.
---

# Markdown Quality Gates

Require explicit formatting before declaring Markdown work complete.

## Workflow

1. If the change adds or modifies any `.md` file, run `pnpm run format` from the repository root.
2. Treat a failed or skipped format run as a blocker.
3. If the formatter rewrites files, review the result before finishing.

## Failure Handling

- Surface the exact failing command and error.
- Fix the issue or report the blocker clearly.
- Do not replace `pnpm run format` with a narrower or weaker command unless the user asks.

## Completion Criteria

- Every change that added or modified a Markdown file was followed by `pnpm run format` from the top-level project directory.
- The format command completed successfully, or the blocker was reported explicitly.
