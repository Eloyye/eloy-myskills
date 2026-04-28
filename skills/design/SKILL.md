---
name: design
description: Use when the user wants stage 3 of the planning workflow. Reads docs/PLANNING.md and docs/SPECIFICATION.md, gathers technical design decisions, and writes docs/DESIGN.md.
---

# Stage 3 - Design

You are helping the developer make concrete technical design decisions for their project. This is stage 3 of 3 (Planning -> Spec -> Design).

## Step 0: Load Prior Context

Before asking anything, read both `docs/PLANNING.md` and `docs/SPECIFICATION.md`.

- If either is missing, stop and say which file is missing and which stage needs to be completed first.
- If both exist, briefly recap: _"I've read your plan and spec for [X]. Let's design the system."_

## Step 1: Gather Design Decisions

Ask questions one group at a time.

**Group A - Architecture**

1. What is the overall architecture? (monolith, microservices, serverless, client-server, etc.)
2. What is the primary tech stack? (languages, frameworks, runtimes)
3. How will the system be deployed and hosted?

After they answer Group A, ask:

**Group B - Key Components** 4. What are the major components or modules in this system? 5. How do they communicate with each other? (API calls, events, queues, direct imports, etc.) 6. Where does state live? (database, cache, in-memory, client, etc.)

After they answer Group B, ask:

**Group C - Design Tradeoffs** 7. What design decisions are you most uncertain about? Walk me through your thinking. 8. Are there any known performance bottlenecks or scaling concerns to design around? 9. What will you build vs. buy vs. borrow (open source)?

After they answer Group C, ask:

**Group D - Developer Experience** 10. How will you run this locally for development? 11. How will you test this? (unit, integration, e2e - and which tooling?) 12. Is there anything about the design that a future developer would find surprising or needs a note?

## Step 2: Confirm Understanding

Summarize the key design decisions back and ask: **"Does this design make sense to you, and are you comfortable moving into implementation?"**

Wait for confirmation.

## Step 3: Write DESIGN.md

Once confirmed, write `docs/DESIGN.md`:

```markdown
# Design

> Stage 3 of 3 - Last updated: {date}
> Based on: [docs/PLANNING.md](./PLANNING.md) - [docs/SPECIFICATION.md](./SPECIFICATION.md)

## Architecture Overview

[High-level description of the architecture and rationale]

## Tech Stack

[Languages, frameworks, libraries, runtimes - with brief justification for each choice]

## Components & Responsibilities

[Table or list of major components and what each one does]

## Data Flow

[How data moves through the system - prose or diagram in ASCII/Mermaid]

## State Management

[Where state lives and why]

## Integration Points

[How this system talks to external services]

## Key Design Decisions

[The most important choices made and why - including tradeoffs]

## Testing Strategy

[Unit / integration / e2e breakdown and tooling]

## Local Development Setup

[How to run this project locally]

## Known Limitations & Future Work

[What is deliberately deferred, and what could be improved later]

## Notes for Future Developers

[Anything surprising, non-obvious, or important to understand about this codebase]
```

## Step 4: Workflow Complete

After writing the file, tell the developer:

> `DESIGN.md` has been written to `docs/DESIGN.md`.
>
> You've completed all three planning stages:
>
> - `docs/PLANNING.md` - Why and what
> - `docs/SPECIFICATION.md` - What exactly
> - `docs/DESIGN.md` - How
>
> These three documents are the source of truth. Reference them as you build.
> Consider committing them before writing code.
>
> You're ready to implement.
