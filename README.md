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

## Files in this guide

| File | Read it when |
|---|---|
| [how-to-prepare.md](how-to-prepare.md) | First. Setup checklist and a two-week plan |
| [part-1-when-ai-gets-it-wrong.md](part-1-when-ai-gets-it-wrong.md) | Before practising Part 1: how AI misleads you and how to catch it |
| [part-2-building-features.md](part-2-building-features.md) | Before practising Part 2: context, planning, reviewing diffs, tests |
| [during-the-session.md](during-the-session.md) | The day before: what to say, timing, common mistakes |
| [your-own-ai-experience.md](your-own-ai-experience.md) | Before any conversation about how you use AI in your own projects |
| [drills.md](drills.md) | Short daily practice on any codebase |
| [self-review.md](self-review.md) | After every mock: score yourself and plan what to practise next |

## The one-sentence version

Treat every AI answer as a claim from a fast, confident junior colleague:
useful, often right, sometimes wrong in ways that look right. Your job is to
know which, and to be able to show why.
