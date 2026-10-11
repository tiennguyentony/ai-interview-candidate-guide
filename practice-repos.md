# Practice repos

These are the ten codebases used in Ace Mentorship mock interviews. Each is a
small, realistic project with its own `EXERCISES.md`. Your mentor will tell
you which one to use for each mock.

To get the most from a mock, **don't explore the repo chosen for your mock
before the session**: exploring unfamiliar code is the skill being tested.
It's fine to make sure you can install and run one or two of them, so setup
isn't a surprise.

| Repo | Stack | What it is |
|---|---|---|
| [courtbook](https://github.com/tiennguyentony/courtbook) | TypeScript, Fastify, SQLite | Booking sports courts at venues: holds, confirmations, cancellations, waitlists |
| [shopcart](https://github.com/tiennguyentony/shopcart) | Python, FastAPI, SQLite | An online shop: products, cart, coupons, checkout, orders |
| [eventpass](https://github.com/tiennguyentony/eventpass) | TypeScript, Express (in-memory store) | Meetup events: RSVPs, recurring events, webhooks, attendee export |
| [splitpot](https://github.com/tiennguyentony/splitpot) | TypeScript, Hono, SQLite | Splitting shared expenses among friends, with currencies and settle-up |
| [recipebox](https://github.com/tiennguyentony/recipebox) | React, TypeScript, Vite (frontend) | A recipe app: search, serving scaler, favourites, shopping list |
| [shortlink](https://github.com/tiennguyentony/shortlink) | Python, FastAPI, SQLite | A URL shortener with click analytics and API keys |
| [jobrunner](https://github.com/tiennguyentony/jobrunner) | Python (standard library only) | A background job queue: workers, retries, scheduling, a CLI |
| [chatterbox](https://github.com/tiennguyentony/chatterbox) | TypeScript, WebSocket | Chat rooms: live messages, history, unread counts |
| [gradebook](https://github.com/tiennguyentony/gradebook) | Python, Flask, SQLite | A school gradebook: weighted grades, late penalties, CSV import |
| [logscope](https://github.com/tiennguyentony/logscope) | Python command-line tool | Analysing web-server access logs: summaries, latency percentiles, filters |

## Setup

Every repo's `EXERCISES.md` has exact instructions. In general:

**TypeScript repos** (courtbook, eventpass, splitpot, recipebox, chatterbox),
Node.js 24 recommended (22.13+ works):

```bash
git clone https://github.com/tiennguyentony/<repo>.git
cd <repo>
npm install
npm test
npm run dev
```

**Python repos** (shopcart, shortlink, jobrunner, gradebook, logscope),
Python 3.11+ and [uv](https://docs.astral.sh/uv/):

```bash
git clone https://github.com/tiennguyentony/<repo>.git
cd <repo>
uv sync
uv run pytest
```

Then follow the repo's `EXERCISES.md` to run the app, CLI or demo. Without
uv, use `python -m venv .venv`, activate it, and `pip install -e ".[dev]"`.

## How every repo's exercises are organised

- **Warm-up:** explore with AI, then explain the code with the AI window
  closed.
- **Part 1: When AI gets it wrong:** tickets, AI-written output to review,
  docs to check, missing tests to write.
- **Part 2: Build a new feature with AI:** a small, a medium and a larger
  feature, then an `AGENTS.md` and a PR description.

See [part-1-when-ai-gets-it-wrong.md](part-1-when-ai-gets-it-wrong.md) and
[part-2-building-features.md](part-2-building-features.md) for how to
approach each part.
