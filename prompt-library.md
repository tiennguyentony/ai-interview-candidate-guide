# Prompt library

Prompts to copy and adapt. Replace the `<...>` parts. In every case,
**attach or paste the relevant files**: the AI can't check code it hasn't
seen.

Notice the pattern: good prompts ask the AI to **show its evidence** (file
and line), to **separate facts from guesses**, and to **stop before writing
code** until you've agreed on the plan.

## Exploring a new codebase

**Map the repo**

> Here is the file tree and the README. Give me a map of this codebase: the
> entry point, the main layers, where business rules live, where data is
> stored, and any background jobs. For each claim, name the file it comes
> from. Mark anything you inferred from the README rather than from code.

**Trace one flow**

> Trace what happens when `<POST /orders>` is called, from the route to the
> database and back, including errors. List each function in order with its
> file. Then list anything that happens later without another request
> (jobs, expiry, retries, emails).

**Find where a rule is enforced**

> Where is the rule "<an email address can only belong to one account>" enforced?
> Check the code, the database schema and migrations, middleware and
> config. Quote the exact lines. If you can't find it, say so; don't guess.

**Check docs against code**

> Here is the README and the config/startup code. List every factual claim
> in the README (stack, env variables, commands, features) and say whether
> the code confirms it, contradicts it, or doesn't mention it. Quote the
> line for each.

## When something is wrong

**Ask for hypotheses, not a fix**

> Symptom: <what happens>. Expected: <what should happen>. Steps to
> reproduce: <...>. Here are the files on that code path. Give me up to
> three possible causes, ranked, with the evidence for each. Don't write a
> fix yet.

**Write a failing test first**

> Write one test that reproduces this bug: <description>. The test must fail
> on the current code for the right reason. Use the existing test helpers in
> `<tests/helpers>`. Don't change any non-test code.

**Triage a bug list**

> You listed these possible problems: <paste>. For each one, say whether
> it's definitely real, possibly real, or not a problem, and what would
> prove it. Then rank the real ones by impact on users (data loss and
> security first).

**Check a claim the AI made**

> You said "<passwords are hashed with bcrypt>". Show me the exact line that
> does it. If you can't find one, say so.

## Building a feature

**Plan first**

> Here is context.md and the files I think are relevant. Propose a plan in
> 4–6 small steps for <feature>. For each step: files to change, what to
> reuse, and which tests prove it. Point out anything in the task that's
> ambiguous. Don't write code yet.

**One step at a time**

> Implement step <2> only. Reuse `<existing function>` instead of writing a
> new one. Follow the error-handling pattern in `<file>`. Don't touch any
> other files. Write the tests first: <cases>.

**Review my diff**

> Review this diff as a strict senior engineer. Check: does every function
> it calls exist? Does it duplicate existing logic? Does it break any of
> these invariants: <list>? Are permissions checked? Do the tests check
> behaviour or just status codes? List problems by severity, with line
> references.

**Edge cases**

> What edge cases could break <function>? Think about empty input,
> boundaries, duplicates, concurrent calls, time zones, permissions and
> failures of external calls. For each, give a test input and expected
> result.

## Wrapping up

**Draft the PR description (then edit it)**

> Draft a PR description for this diff with sections: What changed, How I
> verified it, What I did NOT verify, Where to review closely, AI use. Only
> claim tests that appear in the diff. I'll correct it.

**Rules file**

> Based on the mistakes in this conversation, draft an AGENTS.md of at most
> 30 lines: how to run tests, conventions, invariants and where they're
> enforced, outdated docs to ignore, things never to do. Only include
> rules that are specific to this repo and true.

## Prompts to avoid

| Instead of | Try |
|---|---|
| "Fix the bug." | The symptom, the expected behaviour, the files, and "what could cause this?" |
| "Build the feature." | "Propose a plan. Don't write code yet." |
| "Find all the bugs." (then trusting the list) | Ask, then triage and prove each item yourself |
| "Act as a senior engineer..." with no context | Give the actual files and rules; role phrases add little |
| "Are you sure?" | "Show me the line that proves it." |
| A 400-line paste with no question | Only the relevant code, plus a specific question |
