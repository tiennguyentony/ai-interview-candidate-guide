# How to prepare

## Setup checklist (do this at least a day before)

A broken setup can eat the first 15 minutes of a session. Check all of this
on the machine you will use.

- [ ] **Git** installed, and you can clone from GitHub.
- [ ] **Node.js 24** (22.13 or newer works): `node -v`
- [ ] **Python 3.11+**: `python --version`
- [ ] **uv** for Python projects: `uv --version`
      (install from https://docs.astral.sh/uv/, or be ready to use
      `python -m venv` and `pip`).
- [ ] An editor you are fast in, with search across files.
- [ ] Your AI tool, logged in, and you know how to:
  - attach or paste files into it,
  - start a fresh conversation,
  - **export or copy the whole conversation** (you'll be asked for it).
- [ ] Screen sharing works with your AI window visible.
- [ ] A free-tier chat tool as a backup in case your main tool is down.

Dry run: clone any small public repo, install it, run its tests, start it,
and ask your AI tool to explain one file. If any step is slow, fix it now.

## A two-week plan

About 45 minutes a day. Adjust to your schedule.

### Week 1: build the habits

| Day | Practice |
|---|---|
| 1 | Read the README, this file and [worked-example.md](worked-example.md). Do the setup checklist, and install one repo from [practice-repos.md](practice-repos.md) to be sure your setup works. |
| 2 | Pick a small open-source repo you don't know. Spend 20 minutes exploring it with AI, then write a one-page explanation *without* AI. Compare. (See [drills.md](drills.md), drill 1.) |
| 3 | Read [part-1-when-ai-gets-it-wrong.md](part-1-when-ai-gets-it-wrong.md). Ask AI to "find bugs" in the same repo. For each finding, decide real / not real and write down how you checked. |
| 4 | Practice exercises 1–3 in [practice/](practice/README.md), then check [practice/solutions.md](practice/solutions.md). Skim [concepts.md](concepts.md). |
| 5 | Practice exercises 4–6. Read [part-2-building-features.md](part-2-building-features.md). Write a `context.md` for your open-source repo before asking AI for anything. |
| 6 | Practice exercises 7–8. Build one small feature in your open-source repo with AI, using [prompt-library.md](prompt-library.md). Review every line of the diff. Write a PR description. |
| 7 | Rest, or read [your-own-ai-experience.md](your-own-ai-experience.md) and collect your evidence. |

### Week 2: practise under interview conditions

| Day | Practice |
|---|---|
| 8 | A full timed session on a different open-source repo: 10 min explore, 20 min Part 1 style, 25 min Part 2 style, 5 min PR description. Record your screen. |
| 9 | Watch your recording with [self-review.md](self-review.md). Pick the one habit that would help most. |
| 10 | Drills from [drills.md](drills.md) targeting that habit. |
| 11 | **Mock interview** with a mentor (they choose the practice repo). |
| 12 | Self-review the mock. Compare your scores with your mentor's. |
| 13 | Drills on your weakest area. Prepare your stories for the AI-experience conversation. |
| 14 | Light review of [during-the-session.md](during-the-session.md) and [cheat-sheet.md](cheat-sheet.md). Check your setup again. |

## About the practice repos

The mock interviews use ten small practice repos, listed with setup
instructions in [practice-repos.md](practice-repos.md). Your mentor picks
one for each mock.

To get real value from your mocks:

- **Don't** explore the repo chosen for your mock beforehand, and don't read
  other students' notes or solutions. If you already know the answers, the
  mock can't tell you or your mentor anything useful. Installing a repo to
  check your setup is fine.
- **Do** practise on open-source repos of similar size (1,000–3,000 lines,
  with tests): small libraries in your main language, example apps from
  framework docs, or a classmate's project. And do the exercises in
  [practice/](practice/README.md), which come with answers.
- After a mock, your mentor may send you the debrief. Read it carefully:
  it is the most useful feedback you'll get.

## What to practise in

Use the language you are strongest in for practice, but be ready for
TypeScript or Python. The practice repos use both, and a React frontend and
a command-line tool appear too. You don't need to know every framework; you
need to be able to read unfamiliar code calmly and check things.
