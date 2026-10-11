# Talking about your own AI experience

Some sessions include a conversation about how **you** use AI in your own
projects. Interviewers look for real experience, backed by evidence, and an
honest view of where AI helps and where it doesn't.

## Rule number one: bring receipts

Every story should come with something you can show on screen: a commit, a
diff, a prompt, a chat log, a rules file, a bug ticket. Prepare these before
the session and have the project open.

Interviewers will ask follow-up questions, and they may ask you to explain
an AI-written function line by line. Only bring stories you can defend.

## Prepare these stories

Write a few bullet points for each. Have the evidence ready.

1. **A recent project where AI wrote a meaningful share.**
   - Which tool and mode (autocomplete, chat, agent), for which tasks?
   - Roughly how much of the AI's code survived to the final commit, and
     why the rest didn't?
   - One AI-written function you can explain line by line.
2. **AI was wrong in a way that wasn't obvious.**
   - What kind of mistake (invented API, edge case, security, subtle
     behaviour change)?
   - How did you catch it? How far did it get?
   - What do you do differently now? (A specific habit.)
3. **AI-written code that shipped (or was submitted) and was wrong.**
   - Impact, who you told, what you changed.
4. **A time you chose not to use AI, or turned it off.**
   - Why: learning something new, sensitive code or data, irreversible
     changes, checking would take longer than writing?
5. **Your rules or context file** (`CLAUDE.md`, `AGENTS.md`,
   `.cursorrules`, a saved prompt).
   - What did you add after the AI kept getting something wrong? What did
     you remove?
6. **A time the AI made you slower.**

If you don't have some of these yet, spend a week building something small
with AI and keep notes: save one rejected diff, one prompt you rewrote, one
bug the AI introduced, and write and edit a rules file.

## Know your position on these topics

- **Secrets and data:** what you never paste into an AI tool (keys,
  passwords, customer data, private company code) and **how** you make sure
  (gitignored env files, secret scanning, approved tools, retention
  settings).
- **Agents:** what you let an agent do without asking (and what you never
  would), and why text an agent reads from files or the web can contain
  instructions aimed at it (prompt injection).
- **Reviewing AI code:** how you review a large AI-written change
  differently from a colleague's.
- **Disclosure:** whether a PR or take-home should say which parts were
  AI-written. (Usually yes, and you should be able to explain all of it.)
- **Learning:** how you keep learning when AI writes code for you.
- **Packages:** what you check before installing a package the AI suggests.

## Red flags to avoid

- "I use it for everything" or "I never use it", with no reasoning.
- "It's never really been wrong."
- Stories with no evidence, or evidence you can't explain.
- Claiming 0% or 100% of AI code survives.
- Treating prompting as magic phrases ("act as a senior engineer").
- Not knowing what your tool does with your data.

## Good signs to aim for

- Specific stories with a detection method and a lesson.
- Matching the tool to the task.
- A clear boundary for when you don't use AI.
- Habits: reading every diff, failing test first, searching for every new
  import, small steps.
- Honest about weaknesses, including your own.
