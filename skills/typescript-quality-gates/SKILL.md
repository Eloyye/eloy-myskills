---
name: typescript-quality-gates
description: Enforce required verification for TypeScript project work. Use when Codex is building, modifying, refactoring, or debugging a TypeScript codebase, or any project with `tsconfig.json` and TypeScript source. Before finishing, run the project's tests, run the TypeScript typechecker with `tsc`, run Prettier, and run ESLint. Do not suppress, ignore, bypass, or hide errors from any of these checks.
---

# TypeScript Quality Gates

Require explicit validation before declaring TypeScript work complete. Treat failed tests, type errors, formatting issues, and lint findings as real blockers unless the user tells you to stop earlier.

## Workflow

1. Identify the project commands first.
2. Prefer repository scripts over ad hoc commands.
3. Run all four required checks before finishing.
4. If a check fails, fix the problem or report the blocker clearly.
5. Do not claim the work is done while any required check is failing or skipped.

## Docstrings

Add concise, clear, best‑practice JSDoc/TSDoc comments for all new or changed public APIs.

- Use `/** ... */` above exported functions, classes, interfaces, types, and public methods.
- Start with a one‑sentence summary that states purpose and behavior.
- Use tags only when they add value: `@param`, `@returns`, `@throws`, `@deprecated`, `@example`, `@template` (for generics).
- Describe intent, side‑effects, units, and constraints; avoid restating obvious types or names.
- Prefer examples that are short and runnable; omit examples if redundant.
- Document error conditions and thrown exceptions when meaningful.
- For React components, document props and defaults; call out accessibility considerations.
- Keep comments in sync with code; update or remove stale docs.
- Follow any repo ESLint/TSDoc rules; do not disable rules just to pass.

## Command Selection

- Prefer the narrowest project-defined commands that still validate the changed TypeScript code.
- Use workspace or package scripts when they exist, for example `pnpm test`, `pnpm typecheck`, `pnpm format:check`, and `pnpm lint`.
- If no script exists for type checking, run the TypeScript compiler directly with `tsc --noEmit` against the relevant `tsconfig.json`.
- If no script exists for formatting, run Prettier in checking mode first, for example `prettier . --check` or a scoped equivalent.
- If no script exists for linting, run ESLint on the relevant files or package without reducing rule severity.

## Required Checks

Run these checks in this order unless the repo has a stronger established workflow:

1. TypeScript typecheck through `tsc`
2. Tests
3. Prettier
4. ESLint

## Failure Handling

- Surface the real failing command and the actual error.
- Fix issues by changing code or configuration deliberately, then rerun the failing check.
- If a command cannot run because the project is missing setup or has an unrelated existing failure, say so explicitly.
- Do not replace a failed check with a weaker one just to get a passing result.

## Do Not Suppress Errors

- Do not use `|| true`, `; exit 0`, or similar patterns that hide failures.
- Do not add ignore files, disable comments, or downgrade rules solely to make checks pass.
- Do not skip `tsc` in favor of framework-only diagnostics when TypeScript compilation is available.
- Do not report success if any required check was not run.

## Completion Criteria

Before finishing, confirm all of the following:

- Tests ran successfully.
- TypeScript type checking ran successfully.
- Prettier ran successfully.
- ESLint ran successfully.
- Concise JSDoc/TSDoc docstrings exist for all new or changed public APIs.
- Any remaining unverified work or blocked command is called out explicitly.
