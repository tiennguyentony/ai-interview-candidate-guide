# Part 1: When AI gets it wrong

In Part 1 the codebase contains things that commonly fool AI tools. Your job
is not to find every problem. It is to show that you **check** what the AI
tells you and can **prove** what is actually true.

## How AI tools mislead you

These patterns come up again and again. Learn to expect them.

| Pattern | What it looks like | How to catch it |
|---|---|---|
| **Trusting docs and comments** | AI repeats the README or a code comment, even when the code says something else | Treat docs as history and code as truth. Check config files, tests and the code path itself. `git log` on a file shows when docs and code drifted apart. |
| **Inventing things** | Calls a function, method, flag or package that doesn't exist, or the wrong version's API | Search the codebase for every new name it uses. Run the type checker and tests. Check package registries before installing anything. |
| **Only reading part of the system** | "There is no check for X" when X is enforced somewhere else (database constraints, middleware, config) | Look at migrations/schema, framework hooks and config, not just the main source files. Ask "where *else* could this be handled?" |
| **Calling good design a bug** | Suggests "fixing" something that was a deliberate choice | Look for an explanation: design docs, ADRs, comments, tests that pin the behaviour. Ask what would break if you "fixed" it. |
| **Mirror tests** | Writes tests that assert whatever the code does today, so the tests pass even when the code is wrong | Write the expected value yourself, from the requirement, before looking at the output. Break the code on purpose and check the test fails. |
| **Long confident lists** | "Here are 12 bugs" where half are style notes or not real | Triage: real or not, how bad, who is affected. Prove the top one. |
| **Fixing the symptom** | Makes the error go away without explaining the cause | Ask for the cause before the fix. Reproduce first, then confirm the cause with a test or log line. |
| **Classic blind spots** | Time zones and daylight saving, money as floats, rounding, concurrency and race conditions, off-by-one, pagination, caching, permissions | When code touches these areas, slow down and test edge cases yourself. |

## A method for bug tickets

You may get several tickets written like real customer or team messages. At
least one is often **not a bug**: the system is doing what it was designed
to do.

For each ticket:

1. **Reproduce.** Run the app, call the endpoint, or write a test that shows
   the reported behaviour. If you can't reproduce it, say so.
2. **Classify.** Real bug / not a bug (works as designed) / can't reproduce.
   For "not a bug", point at the evidence: a design doc, a test, a comment
   that explains the intent.
3. **Prove.** A failing test is the best proof. A `curl` command or a
   database query also works.
4. **Fix** (if asked and it's real). Then show the failing test now passes
   and the rest of the suite still passes.
5. **Rank.** If you can't do all tickets, say which matters most and why:
   who is affected, how many, can they lose money or data?

## When the AI gives you a list of problems

Interviewers often ask: *"It listed ten things. Which are real?"*

- Go through each one quickly: real / not real / not sure.
- For "not real", have a one-line reason.
- Rank the real ones by impact (data loss and security usually first).
- Prove the top one. One proven problem beats ten unproven ones.

## When you review AI-written output

Some exercises give you something the AI already wrote: a bug report, an
explanation, a test file or a pull request. Read it the way you would review
a colleague's work:

- Check each claim against the code. Mark it true / false / partly true.
- For a diff: does every function it calls exist? Does it change anything
  unrelated? Do the tests actually test the change? Does the description
  match the diff?
- Don't stop at the first problem. AI output often has several.
- Write down what you'd tell the author, as review comments.

## Verification toolkit

Get fast at these. They are what separates "I think" from "I checked".

- **Search the code:** your editor's global search, or `grep -rn "name" src`.
- **Find callers:** search for where a function is used, not just defined.
  A function that exists but is never called is a common finding.
- **Read the schema:** migrations and database constraints often hold the
  most important rules.
- **Run the app:** what you see on first run often shows a problem the AI
  never mentions.
- **Write a failing test first:** it proves the bug and later proves the fix.
- **Break it on purpose:** change one line and check your test goes red.
- **Check history:** `git log -p <file>` shows when and why something
  changed.
- **Run the full suite and the type checker** before saying "done".

## Things to say out loud

- "The AI says X. Before I believe it, let me check where that comes from."
- "This comment says one thing; the config says another. I'll trust the
  code and the test."
- "I'm going to write a failing test first so we can see the bug."
- "I don't think this one is a bug. Here is the design note that explains
  it."
- "The AI found eight things. I think three are real. The worst is this one,
  because..."
