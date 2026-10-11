# Drills

Short exercises to do on **any** codebase: an open-source project, a
classmate's repo, or your own old project. 10–15 minutes each. Do one or two
a day in the two weeks before a mock.

## Drill 1: Explore, then explain without AI (15 min)

1. Spend 10 minutes exploring the repo with AI.
2. Close the AI. In 5 minutes, write down: what the system does, the main
   flow of one request or command with file and function names, and one
   rule it must never break.
3. Reopen the AI and the code. What did you get wrong? What did the AI tell
   you that wasn't true?

## Drill 2: Docs vs code (10 min)

Pick a README or a doc comment. List five factual claims (stack, env
variables, commands, features, defaults). Check each against the code.
Mark true / false / outdated, with the line that proves it.

## Drill 3: Is this test meaningful? (10 min)

Pick three existing tests. For each, change one line of the code it covers
so the behaviour is wrong. Does the test fail? If not, why not? Then revert.

Variant: ask the AI to write a test for a function, then do the same check.

## Drill 4: Verify an AI claim (10 min)

Ask the AI a factual question about the repo ("Where is X validated?",
"What happens when Y fails?", "Is Z thread-safe?"). Then prove or disprove
the answer yourself with search, a test or by running the code. Keep a
tally: how often was it fully right?

## Drill 5: Triage an AI bug list (15 min)

Ask the AI to find bugs in one module. For each finding: real / not real /
unsure, and one line of reason. Rank the real ones. Prove the top one with a
failing test.

## Drill 6: Spot the invented code (10 min)

Ask the AI to add a small change that uses a library you know well. Before
running it, check every function, argument and import against the library's
documentation or source. Note anything that doesn't exist or is from a
different version.

## Drill 7: Write the context first (10 min)

Pick a small feature idea. Before any prompt, write a `context.md` (see
[part-2-building-features.md](part-2-building-features.md)). Then send the
same request twice: once with only the task text, once with your context
file. Compare the two diffs.

## Drill 8: Review a diff out loud (10 min)

Take any recent AI-generated diff (or a real pull request on an open-source
project). Record yourself reviewing it out loud using the checklist in Part
2, step 5. Watch it back: did you check that things exist? Did you look at
the tests?

## Drill 9: Classic blind spots (15 min)

Pick one area and write tests for it in any function that touches it:

- dates near midnight, across time zones, across a daylight-saving change;
- money: rounding, splitting an amount three ways, currency conversion;
- pagination: first page, last page, items with equal sort keys;
- two requests at the same time doing the same thing;
- one user trying to read or change another user's data.

## Drill 10: Honest PR description (5 min)

After any coding session, write the PR description with the "What I did
NOT verify" section filled in properly. If that section is empty, you
probably didn't look hard enough.

## Drill 11: Rules file (10 min)

After a session where you corrected the AI more than once, write the
`AGENTS.md` lines that would have prevented it. Keep only lines that are
specific and true.

## Drill 12: Explain-back (10 min)

Pick one AI-written function from your own work. Explain it line by line to
a friend, a rubber duck or a recording. Wherever you hesitate, that's a part
you don't really understand yet. Read the docs for it.
