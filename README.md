# Candidate guide: AI-assisted coding interviews

This guide helps you prepare for Ace Mentorship mock interviews, and for real
interviews at companies that let you use AI tools.

In these interviews you get a codebase you have never seen and you use
Claude, ChatGPT, Copilot, Cursor or any tool you like. The interviewer is not
testing whether you can get the AI to produce code. They are testing whether
you can **work with** it: give it the right context, catch it when it is
wrong, and stand behind the result.

## What the session looks like

You clone a small practice repo and follow its `EXERCISES.md`. Every repo has
the same shape:

| Part | What you do | What it shows |
|---|---|---|
| **Warm-up** | Explore with AI, then explain the code with the AI window closed | You understand the code, not just the AI's summary |
| **Part 1: When AI gets it wrong** | Work through tickets, review AI-written output, check docs, write missing tests | You catch confident wrong answers and prove what is true |
| **Part 2: Build a new feature with AI** | Plan, give context, build, test, write an `AGENTS.md` and a PR description | You can ship something real with AI without breaking things |

Sessions last 45–90 minutes. You will not finish everything, and that's
expected. Saying clearly what you skipped and why counts in your favour.

## What interviewers score

Five areas, each from 1 to 4. They are not averaged.

1. **Reading code:** you can explain what the code does, with file and
   function names, without the AI.
2. **Judgment:** you rank risks sensibly, recognise good design, and know
   when not to trust the AI.
3. **Prompting and context:** you give the AI the right files, rules and
   definition of done, and break big tasks into steps.
4. **Verification:** you read diffs, check that things exist, and prove
   claims with tests or commands. **This one matters most.** A weak score
   here means "not ready" whatever the other scores are.
5. **Honesty:** you say what you didn't check, don't overclaim, and are
   open about what the AI wrote.

## Start here

1. Read this page and [worked-example.md](worked-example.md) (10 minutes)
   to see what interviewers notice.
2. Follow the two-week plan in [how-to-prepare.md](how-to-prepare.md).
3. The day before a mock, read [during-the-session.md](during-the-session.md)
   and keep [cheat-sheet.md](cheat-sheet.md) open.
4. After every mock, fill in [self-review.md](self-review.md).

## Everything in this guide

| File | What it's for |
|---|---|
| [how-to-prepare.md](how-to-prepare.md) | Setup checklist and a two-week plan |
| [practice-repos.md](practice-repos.md) | The ten practice repos used in mocks, with setup commands |
| [worked-example.md](worked-example.md) | The same ticket handled by a weak and a strong candidate, with what the interviewer sees |
| [part-1-when-ai-gets-it-wrong.md](part-1-when-ai-gets-it-wrong.md) | How AI misleads you, and how to catch it and prove what's true |
| [part-2-building-features.md](part-2-building-features.md) | Context, planning, prompts, reviewing diffs, tests, PR descriptions |
| [practice/](practice/README.md) | Eight self-check exercises with [solutions](practice/solutions.md) |
| [prompt-library.md](prompt-library.md) | Prompts to copy for exploring, debugging, planning and reviewing |
| [concepts.md](concepts.md) | Topics AI often gets wrong: time zones, money, races, auth, injection and more |
| [during-the-session.md](during-the-session.md) | What to say, timing, common mistakes |
| [cheat-sheet.md](cheat-sheet.md) | One page to keep beside you |
| [your-own-ai-experience.md](your-own-ai-experience.md) | Preparing to talk about how you use AI in your own projects |
| [drills.md](drills.md) | Twelve 10–15 minute drills for any codebase |
| [self-review.md](self-review.md) | Score yourself after a mock and plan what to practise next |
| [faq.md](faq.md) | Common questions |

## The one-sentence version

Treat every AI answer as a claim from a fast, confident junior colleague:
useful, often right, sometimes wrong in ways that look right. Your job is to
know which, and to be able to show why.
