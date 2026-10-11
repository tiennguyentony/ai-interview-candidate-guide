# During the session

## The first five minutes

1. Share your screen, **including the AI chat window**.
2. Clone the repo, install, run the tests. If something fails, say so
   straight away. The interviewer would rather help than watch you fight it.
3. Skim `README.md` and `EXERCISES.md`. Ask which exercises to do and how
   long you have.
4. Start the app (or run the CLI) once. Look at what it shows.

## Think out loud

The interviewer can't score what they can't see. Say:

- what you're looking for and why ("I'm searching for where this is
  called"),
- what you're asking the AI and why,
- whether you **accept, edit or reject** each AI answer, and why,
- when you're unsure ("I'm not sure yet; let me check").

Silence while the AI types is fine. Silence while you accept its output is
not.

## Explaining with the AI window closed

At some point you'll be asked to close the AI and explain the code. To be
ready:

- While exploring, open the actual files the AI mentions. Follow one request
  or command from start to finish yourself.
- Keep short notes with **file and function names**.
- Aim to explain: what the system does, the main flow step by step, what
  happens later with no further input (background jobs, expiry, retries),
  and the rules it must never break.

## Timing

- Respect the suggested times in `EXERCISES.md`. If you're stuck for more
  than five minutes, say what you've tried and move on, or ask for a hint.
- In Part 2, aim for something small that works and is tested, then extend.
- Keep five minutes at the end for the PR description.

## Ground rules you'll be asked to follow

- Keep your AI conversation and save it to `session/ai-transcript.md` (or
  share a link) at the end.
- Work on a branch called `session/<your-name>`.
- Don't paste the interviewer's questions into the AI while you're
  explaining.

## Common mistakes

| Mistake | Do this instead |
|---|---|
| Accepting AI output without reading it | Read every diff. Say what you checked. |
| "The tests pass, so it's correct" | Ask whether the tests would fail if the code were wrong. |
| Repeating what the README says | Check the README against the code. |
| Fixing everything the AI lists | Triage, rank, prove the top item. |
| "Fixing" a deliberate design choice | Look for the reason first (design docs, tests). |
| One giant prompt for the whole feature | Context file, plan, small steps. |
| Going silent | Narrate your reasoning. |
| Claiming more than you did | Say exactly what you ran and what you didn't. |
| Panicking when the AI is wrong | That's the point of the exercise. Catching it is a win. |

## If your AI tool fails

Switch to a backup chat tool and paste files manually. Interviewers score
what you do with the output, not which tool you use or how fancy it is.

## At the end

- Export the AI transcript.
- Commit your work to your branch.
- Write the PR description honestly.
- Ask your interviewer: "What's the one thing I should practise next?"
