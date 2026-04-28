---
name: python-quality-gates
description: Enforce required verification for Python project work. Use when Codex is building, modifying, refactoring, or debugging Python code, or any project with Python source. Before finishing, run type checking with `ty`, linting/format checks with `ruff`, and tests in that order. Do not suppress, ignore, bypass, or hide errors from any of these checks.
---

# Python Quality Gates

Require explicit validation before declaring Python work complete. Treat type errors, lint findings, format issues, and failed tests as blockers unless the user tells you to stop earlier.

## Workflow

1. Identify the project commands first.
2. If the current checkout is a git worktree other than the main checkout, run `uv sync` from the main checkout's `apps/worker/python` directory before running Python quality checks.
3. Prefer repository scripts over ad hoc commands.
4. Run the required checks in order: `ty`, `ruff`, then tests.
5. If a check fails, fix the problem or report the blocker clearly.
6. Do not claim the work is done while any required check is failing or skipped.

## Docstrings

Add concise, clear, best-practice Python docstrings for all new or changed public APIs.

- Use triple double-quoted docstrings on public modules, classes, functions, methods, and dataclasses when the behavior is not self-evident.
- Start with a one-sentence summary that states what the API does.
- Keep docstrings short by default; prefer one sentence unless parameters, return values, side effects, or exceptions need clarification.
- Document units, invariants, side effects, and raised exceptions when they are important to correct use.
- Document parameters only when their meaning, units, valid range, lifecycle, or relationship to other parameters is non-obvious.
- Document return values when their semantics matter beyond the type annotation.
- Document side effects and exceptions whenever callers must plan around them.
- Avoid restating names or types that are obvious from annotations.
- Do not add noisy docstrings to private helpers or simple tests unless they clarify non-obvious behavior.
- Keep docstrings synchronized with implementation changes.

Good example:

```python
def wait_for_object(store: ObjectStore, bucket: str, key: str, attempts: int) -> None:
    """Wait until an object is visible in storage, or raise after the retry budget expires."""
    for _ in range(attempts):
        if store.exists(bucket, key):
            return
        time.sleep(1)

    raise FileNotFoundError(f"{bucket}/{key} was not available before timeout")
```

Good example for a public API where callers need more detail:

```python
def extract_frames(video: Path, output_dir: Path, fps: int | str) -> int:
    """Extract JPEG frames from a normalized video.

    Args:
        fps: Detection frames per second, or `"native"` to preserve source cadence.

    Returns:
        Number of frame files written to `output_dir`.

    Raises:
        CalledProcessError: If ffmpeg cannot decode the input video.

    Side effects:
        Creates `output_dir` and writes `frame-*.jpg` files.
    """
```

## Command Selection

- Prefer the narrowest project-defined commands that still validate the changed Python code.
- If the current checkout is a non-main git worktree, use `git worktree list` to find the main checkout and run `uv sync` from that checkout's `apps/worker/python` directory before `ty`, `ruff`, or `pytest`.
- Use project commands when they exist, for example `uv run ty check`, `uv run ruff check`, `uv run ruff format --check`, and `uv run pytest`.
- If using `uv`, pass the project root when needed, for example `uv run --project path/to/python ty check`.
- If the default `uv` cache is not writable in the sandbox, set a writable cache such as `UV_CACHE_DIR=/tmp/uv-cache` or use `--cache-dir /tmp/uv-cache`.
- If no repository command exists, run the tools directly against the relevant package or files.
- Do not replace a stronger project command with a weaker ad hoc command just to get a passing result.

## Required Checks

Run these checks in this order unless the user explicitly asks for a different sequence:

1. Type checking: `ty check`
2. Linting and formatting: `ruff check`, plus `ruff format --check` when Ruff formatting is used by the project
3. Tests: `pytest` or the repository's Python test command

## Failure Handling

- Surface the failing command and the actual error.
- Fix issues deliberately, then rerun the failed command before continuing.
- If a command cannot run because setup is missing or an unrelated existing failure blocks it, say so explicitly.
- Do not skip tests because `ty` or `ruff` passed.
- Do not report success if any required check was not run.

## Do Not Suppress Errors

- Do not use `|| true`, `; exit 0`, or similar patterns that hide failures.
- Do not add blanket `# type: ignore`, `# noqa`, `# pyright: ignore`, or config downgrades solely to pass gates.
- Do not weaken tests or assertions to make the suite green.
- Do not ignore generated files by broadening config unless the files are genuinely generated or third-party artifacts.

## Completion Criteria

Before finishing, confirm all of the following:

- `ty` ran successfully.
- `ruff` linting and any configured Ruff format check ran successfully.
- Tests ran successfully.
- Concise Python docstrings exist for all new or changed public APIs where they add value.
- Any remaining unverified work or blocked command is called out explicitly.
