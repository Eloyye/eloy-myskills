---
name: spec
description: Use when the user wants stage 2 of the planning workflow. Reads docs/PLANNING.md, gathers functional and technical specification details, and writes docs/SPECIFICATION.md.
---

# Stage 2 - Specification

You are helping the developer write a precise technical specification for their project. This is stage 2 of 3 (Planning -> Spec -> Design).

## Step 0: Load Prior Context

Before asking anything, read `docs/PLANNING.md`. If it does not exist, stop and say:

> `docs/PLANNING.md` was not found. Complete the planning stage first.

If it exists, briefly acknowledge what was planned: _"I've read your plan for [X]. Let's turn that into a specification."_

## Step 1: Gather Specification Details

Ask questions one group at a time, waiting for answers before moving on.

**Group A - Functional Requirements**

1. What are the core user-facing actions or flows? (What can a user do?)
2. Are there any non-functional requirements? (Performance, uptime, accessibility, security, etc.)
3. What are the edge cases or error states you need to handle?

After they answer Group A, ask:

**Group B - Data & Interfaces** 4. What data does this system create, read, update, or delete? 5. Are there external systems, APIs, or services this must integrate with? 6. Who or what are the actors in this system? (users, admins, automated jobs, etc.)

After they answer Group B, ask:

**Group C - Acceptance Criteria** 7. How do you define done for each major piece? What would a passing test look like? 8. Are there any regulatory, compliance, or legal requirements to factor in? 9. Is there anything from the planning stage that you want to revisit or change now that you're thinking more concretely?

## Step 2: Confirm Understanding

Summarize the specification scope back in a short list and ask: **"Does this feel complete as a spec, or are there gaps?"**

Wait for confirmation.

## Step 3: Write SPECIFICATION.md

Once confirmed, write `docs/SPECIFICATION.md`:

```markdown
# Specification

> Stage 2 of 3 - Last updated: {date}
> Based on: [docs/PLANNING.md](./PLANNING.md)

## Functional Requirements

[Numbered list of what the system must do]

## Non-Functional Requirements

[Performance, security, accessibility, reliability targets]

## Actors

[Who or what interacts with the system]

## Data Model (High-Level)

[Key entities and their relationships - prose or simple table]

## Integrations & Dependencies

[External systems, APIs, or services]

## Edge Cases & Error Handling

[Known edge cases and how they should be handled]

## Acceptance Criteria

[Testable conditions for each major feature or flow]

## Open Questions

[Anything unresolved that the design stage should address]
```

## Step 4: Stage Gate

After writing the file, tell the developer:

> `SPECIFICATION.md` has been written to `docs/SPECIFICATION.md`.
>
> Review it carefully; this document is the contract for what gets built.
> Make any edits directly in the file.
> When you're ready, use the `design` skill to begin the design stage.
>
> Do not proceed until you're confident in the spec.
