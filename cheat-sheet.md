# Cheat sheet

One page to keep beside you during practice. Print it or keep it on a
second screen.

## Start (5 min)

- [ ] Share screen **with the AI window visible**
- [ ] Clone, install, run tests, start the app
- [ ] Skim README + EXERCISES. Ask which exercises and how long
- [ ] Branch: `session/<your-name>`

## Every time the AI answers, ask yourself

1. **Where did it get that?** Code, comment, README, or nowhere?
2. **Does it exist?** Every function, file, flag and package it names
3. **Can I prove it?** Test, command, query, or the exact line
4. **Accept, edit or reject?** Say which, and why, out loud

## Part 1: When AI gets it wrong

- Docs are history; **code and tests are truth**
- Look beyond source files: **schema, migrations, config, middleware**
- Reproduce → classify (bug / not a bug / can't reproduce) → **prove** → fix
- At least one ticket may be "works as designed". Look for the reason
- AI lists: **triage, rank, prove the top one**
- Tests: **would it fail if the code were wrong?** Break it to check
- Blind spots: time zones, money, rounding, concurrency, off-by-one,
  pagination, caching, permissions, swallowed errors

## Part 2: Building with AI

- **Ask** about anything ambiguous; state your assumptions
- **`context.md` first:** rules, invariants, code to reuse, definition of done
- **Plan** in small steps; ask for the plan before code
- **Review every diff:** things exist, reuse not rewrite, permissions,
  transactions, no unrelated changes, tests check behaviour
- Run **tests + type checker** before saying "done"
- Out of time? Say what works, what's missing, and the risk

## PR description

What changed · How I verified it · **What I did NOT verify** · Where to
review closely · Which parts were AI-written

## Say out loud

- "Before I believe that, let me check where it comes from."
- "I'll write a failing test first."
- "I don't think this is a bug. Here's why it's designed this way."
- "The AI found eight things; three are real; this one matters most."
- "I didn't check X. That's a risk."

## End

- [ ] Export AI transcript → `session/ai-transcript.md`
- [ ] Commit to your branch
- [ ] Ask: "What's the one thing I should practise next?"
