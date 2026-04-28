---
name: aggressive-refactoring
description: Aggressively refactor code to remove repetition, eliminate deprecated APIs, simplify internal and public APIs, delete dead code, normalize naming and data flow, and reduce unnecessary abstraction while preserving intended behavior. Use when Codex should make a codebase materially cleaner rather than only patching one symptom.
---

# Aggressive Refactoring

Use this skill when cleanup is the goal, not just correctness. Favor high-value structural improvement, but keep the work defensible: behavior should remain intentional, verification should stay explicit, and compatibility should only be preserved when it still serves a real caller.

## Refactoring Targets

Prioritize work that removes long-term maintenance cost:

- Repeated logic that can be expressed once without obscuring behavior
- Deprecated, legacy, or transitional APIs that should be replaced with supported ones
- Overgrown APIs with too many parameters, inconsistent return shapes, or misleading names
- Thin wrappers, pass-through helpers, and abstractions that hide simple logic
- Dead code, unused branches, stale feature flags, compatibility shims, and unused exports
- Confusing naming, duplicate concepts, or split code paths that represent the same idea
- Boilerplate validation, mapping, parsing, or formatting code that can be centralized
- Tests that duplicate implementation detail instead of protecting behavior

Add adjacent cleanup when it is tightly coupled to the same refactor and improves coherence.

## Core Rules

- Optimize for simpler code paths, fewer concepts, and fewer public surfaces.
- Prefer deletion over preservation when code is unused, duplicated, or obsolete.
- Replace deprecated APIs with stable supported equivalents, not newer temporary shims.
- Collapse multiple ways of doing the same thing into one clear path.
- Preserve behavior intentionally, not accidentally. If behavior must change, make it explicit.
- Keep compatibility layers only when a real caller still depends on them.
- Do not keep transitional code "just in case" without evidence.
- Avoid speculative abstraction. Extract only when the commonality is real and improves readability.
- Prefer straightforward control flow and explicit data movement over indirection.
- Keep the result easier to explain than the original code.

## Workflow

### 1. Define the cleanup boundary

- Identify the module, feature area, or API surface to simplify.
- State what kind of debt is being removed: repetition, deprecation, API complexity, stale code, naming drift, or structural churn.
- Check for callers, tests, docs, and generated code that constrain the refactor.
- If the scope is broad, work from the highest-leverage seam outward.

### 2. Inventory the real problems

- Find duplicate logic, not just similar syntax.
- Identify deprecated APIs, legacy adapters, alias functions, and migration leftovers.
- Mark dead code candidates using references, imports, exports, routes, and configuration usage.
- Distinguish essential variation from accidental inconsistency.
- Note where API simplification will force call-site changes; plan those edits together.

### 3. Protect behavior before cutting

- Add or update characterization tests for risky flows before major restructuring.
- For public APIs, ensure current behavior is captured before removing overloads or options.
- If no tests exist, add the smallest coverage that protects the intended behavior.
- Do not start large structural edits with no safety net unless the code is trivial.

### 4. Refactor toward one clear path

- Merge repeated branches into shared helpers or shared control flow.
- Inline abstractions that no longer earn their cost.
- Replace deprecated APIs across the boundary instead of wrapping them again.
- Simplify function signatures: remove unused parameters, reduce boolean switches, prefer cohesive objects only when they clarify meaning.
- Normalize outputs and error handling so callers see one predictable contract.
- Rename symbols when names misrepresent behavior or leak old concepts.
- Delete unreachable, unused, or fully subsumed code as soon as the new path is proven.

### 5. Clean the surrounding surface

- Update call sites, tests, docs, comments, and examples to match the simplified API.
- Remove stale TODOs and migration notes that no longer apply.
- Tighten types, schemas, and validation once the simpler shape is clear.
- Reduce special cases introduced only to support removed legacy paths.

### 6. Verify aggressively

- Run focused tests during the refactor, then broader quality gates before finishing.
- If verification includes Python quality checks and the current checkout is a non-main git worktree, run `uv sync` from the main checkout's `apps/worker/python` directory before those checks.
- If the repo has language-specific quality-gate skills, apply them after the refactor.
- Check that removed APIs have no remaining call sites.
- Confirm error messages, logging, and observability still make sense after deletion and consolidation.

## Heuristics

- Three similar implementations usually want one shared path.
- Two implementations may still justify consolidation if both are actively changing or already diverging incorrectly.
- A helper that is called once is usually just indirection.
- A parameter that only toggles between unrelated behaviors usually wants separate functions.
- If the compatibility layer is more complex than the real implementation, remove or narrow it.
- If a "generic" abstraction requires callers to understand too many hidden rules, make it concrete again.
- If naming cleanup reveals duplicated concepts, merge the concepts instead of only renaming them.

## Do Not Do This

- Do not mix broad behavior changes into a cleanup-only refactor without stating it.
- Do not preserve deprecated APIs forever because updating callers feels inconvenient.
- Do not add a new abstraction layer just to avoid touching several call sites.
- Do not keep dead code behind comments, feature flags, or unreachable branches.
- Do not claim simplification if the implementation became harder to follow.
- Do not stop at local cleanup if the old API shape still infects nearby callers.

## Completion Criteria

Before finishing, confirm all of the following:

- Repetition or deprecated structure was materially reduced, not just rearranged.
- Internal or public APIs are simpler, clearer, or smaller where intended.
- Unused or obsolete code introduced by the old design was deleted.
- Call sites, tests, comments, and docs were updated to the new shape.
- Relevant tests and quality gates ran, or any blocker was reported explicitly.
- The final code has fewer moving parts and a more obvious primary path.
