---
name: tdd-workflow
description: Enforce strict test-driven development when implementing features, fixing bugs, or refactoring behavior. Use when Codex should write or update tests before production code, prove each new test fails for the expected reason, implement the smallest passing change, refactor safely, and finish by running the relevant test suites plus repo validation commands such as lint, format checks, and type checks.
---

# TDD Workflow

Follow a strict red-green-refactor loop. Keep each loop small enough that the cause of failure and the reason for the fix stay obvious.

## Core Rules

- Start from an explicit behavior change, not from an implementation idea.
- Define the smallest observable slice of behavior to add, change, or protect.
- Write or update one test at a time before changing production code.
- Run the narrowest relevant test command immediately after adding the test and confirm it fails for the expected reason.
- Do not implement the feature until the new or updated test is red for the right reason.
- Make the smallest production change that turns the test green.
- Re-run the same focused test after each meaningful code change.
- Refactor only while tests are green.
- Run broader verification after the focused loop is complete.
- Before broader verification that includes Python quality checks, if the current checkout is a non-main git worktree, run `uv sync` from the main checkout's `apps/worker/python` directory.
- Finish by running the repo quality gates that apply: tests, lint, format checks, and static analysis or type checks.

## Workflow

### 1. Define the next slice

- State the behavior in one sentence.
- Prefer one assertion target per loop.
- For bug fixes, write the regression test first.
- If requirements are unclear, stop and ask before coding.

### 2. Write the failing test

- Add the smallest test that demonstrates the intended behavior.
- Keep the test deterministic, isolated, and readable.
- Avoid mocks unless a real dependency makes the test slow, flaky, or hard to control.
- Name the test after the behavior, not the implementation detail.
- If no test harness exists, create the smallest viable harness first, then continue with the same red-green-refactor loop.

### 3. Prove red

- Run the narrowest test command that exercises the new test.
- Verify failure is caused by the missing behavior, not by broken setup or syntax errors.
- If the test passes immediately, strengthen or correct it before touching production code.

### 4. Make it pass

- Change production code only after the red state is confirmed.
- Implement the minimum logic needed to satisfy the failing test.
- Prefer simple code over speculative abstraction.
- Do not broaden the scope mid-loop.

### 5. Refactor

- Clean duplication, naming, and structure only after the test is green.
- Keep behavior constant while refactoring.
- Re-run focused tests after each refactor step.

### 6. Broaden verification

- Run nearby tests that cover the same module or flow.
- Run the full relevant package suite before declaring the task done.
- If that verification includes Python quality gates and the current checkout is a non-main git worktree, run `uv sync` from the main checkout's `apps/worker/python` directory first.
- Run repo-wide validation when the change affects shared code or cross-package behavior.

## Best Practices

- Keep loops short; large test-first batches hide the real cause of failures.
- Test behavior at the right level. Do not force unit tests for behavior that is only meaningful at integration level.
- Prefer one new behavior per commit-sized change.
- Preserve fast feedback by using targeted tests during development and broader suites at the end.
- Avoid rewriting tests to match broken behavior unless the specification changed.
- Do not weaken assertions just to get green.
- When adding branches, add tests for both the expected path and the important edge case.
- When fixing a bug, keep the regression test permanently unless it is redundant with a stronger test.
- If a change is hard to test, treat that as a design signal and simplify the design where practical.
- If refactoring without behavior change, ensure characterization or regression coverage exists first.

## Completion Criteria

Before finishing, verify all of the following:

- Every new behavior or bug fix is covered by a test added before the implementation change.
- Each new test was observed failing before implementation.
- Focused tests pass.
- Broader relevant suites pass.
- `pnpm lint` passes.
- `pnpm format:check` passes, or formatting was fixed and re-checked.
- `pnpm typecheck` passes.
- Any residual risk, missing coverage, or test harness gap is called out explicitly.
