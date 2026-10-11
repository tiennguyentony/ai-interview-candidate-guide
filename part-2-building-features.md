# Part 2: Building a new feature with AI

In Part 2 you build something new. Interviewers watch how you plan, what
context you give the AI, whether you keep the scope under control, and
whether you verify the result. A smaller feature that is correct and well
tested beats a bigger one that you can't explain.

## Step 1: Ask before you build (2 minutes)

Feature tasks often contain a deliberate ambiguity. Reading the task
carefully and asking about it is part of the score.

- What exactly counts as done? Read each acceptance criterion.
- Is anything unclear? ("Should an archived item still appear in the
  list?", "What happens if the same request arrives twice?")
- Who is allowed to do this? Permissions are easy to forget.
- If you can't ask, **write down your assumption** and say it out loud.

## Step 2: Write `context.md` before your first prompt (5 minutes)

The exercises ask you to write `session/context.md` before prompting. This
is the single biggest lever on AI output quality. Include:

```markdown
## What I'm building
One or two sentences, in my own words.

## Codebase rules the AI must follow
- Where things go (routes / services / repositories / domain, or the repo's
  equivalent)
- Conventions: how errors are handled, how time and ids are obtained,
  how money is stored, naming
- Invariants: rules that must never break (e.g. "an email is unique",
  "only the owner can see this")

## Existing code to reuse (don't rewrite)
- file / function: what it does

## Definition of done
- Acceptance criteria from the task
- Tests: which behaviours, including edge cases and permission checks
- `npm test` / `uv run pytest` (and the type checker) pass
- No unrelated files changed

## Out of scope
- What I'm deliberately not doing
```

To fill it in, explore first: find a similar existing feature and copy its
patterns. The best context names real files and functions.

## Step 3: Plan in small steps

Don't ask for the whole feature in one prompt. A good plan:

1. Data or schema change (if any). Never edit a migration that already
   ran; add a new one.
2. Core logic, reusing existing helpers. Unit tests.
3. Wiring: route or command, validation, permissions. Integration tests.
4. Edge cases: errors, empty inputs, concurrent requests, time zones.

Ask the AI for a plan first, review it, *then* ask for code one step at a
time. Commit after each working step.

## Step 4: Prompts that work

**Weak:**

> Add an archive endpoint.

**Strong:**

> Using the rules in context.md (attached), implement step 2 of the plan:
> the service method only, no route yet. Reuse the existing validation
> helper in `domain/…` rather than writing a new one. Follow the error
> pattern in `lib/errors`. Write unit tests first for: normal case, already
> archived, not the owner. Show me the plan for the change before writing
> code.

Good prompts:

- attach or name the relevant files,
- say what to reuse and what not to touch,
- ask for tests and say which cases,
- ask for a plan or for the cause before the fix,
- limit the scope of each step.

## Step 5: Review every diff before you run it

Interviewers will often stop you and say "read me this diff". Check:

- [ ] Every function, method, import and package it uses **exists**.
- [ ] It reuses existing code instead of reimplementing it. (Copying logic
      also copies its bugs.)
- [ ] It respects the invariants in your `context.md`.
- [ ] Permissions: can one user act on another user's data?
- [ ] Multi-step changes that must succeed or fail together are in one
      transaction.
- [ ] Errors are handled, not swallowed.
- [ ] Nothing unrelated changed (formatting, other files, loosened
      validation, edited tests).
- [ ] The tests check behaviour, not just the status code.

Reject or edit at least something. If you accept everything unchanged, the
interviewer will wonder whether you read it.

## Step 6: Tests that actually prove something

- Write expected values yourself, from the requirement.
- Test the edge cases: empty, boundary values, someone else's data,
  duplicates and retries, time near midnight or a daylight-saving change.
- Check a test **can fail**: break the code briefly and watch it go red.
- `expect(status).toBe(200)` alone proves almost nothing.

## Step 7: Teach the next AI (`AGENTS.md` / `CLAUDE.md`)

You may be asked to write a short rules file so the next engineer's AI
doesn't repeat today's mistakes. Keep it under 40 lines. Good lines are
**specific, true, and learned the hard way**:

- How to run tests and the type checker, and "run them before saying done".
- The codebase conventions you had to tell the AI about.
- Invariants and where they are enforced.
- Docs that are out of date, and dead code to ignore.
- Things never to do (e.g. edit applied migrations).

Avoid generic advice like "write clean code".

## Step 8: The pull request description

Write it for a busy senior who has five minutes:

```markdown
## What changed
Short summary, and why.

## How I verified it
Exactly what I ran: tests added, commands, manual checks.

## What I did NOT verify
Be specific. This is where honesty shows.

## Where to review closely
The riskiest part of the diff, and why.

## AI use
Which parts were AI-generated, and what I changed or rejected.
```

The interviewer compares this with what they watched you do. Claiming you
tested something you didn't is the fastest way to lose trust.

## When you run out of time

Stop, and say:

- what works and is tested,
- what is unfinished,
- what the risk would be if this shipped as it is,
- what you would do next.
