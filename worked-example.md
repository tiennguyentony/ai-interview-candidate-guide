# Worked example: the same ticket, two candidates

This is a made-up session on a made-up repo, so you can see what
interviewers notice. The repo is a small notes API (Python, FastAPI,
SQLite). The ticket:

> **#88 from a user:** "I exported my notes to back them up, but the file
> only has 100 notes. I have 340."

---

## Candidate A (weak)

> **A:** *(pastes the ticket into the AI)* "Fix this bug: the notes export
> only includes 100 notes."

> **AI:** "The export probably has a limit of 100. Increase it to 10,000 so
> all notes are included. Here's the change: `LIMIT = 10_000`."

> **A:** "OK, that makes sense." *(finds a `LIMIT = 100` constant, changes
> it, runs the tests, all green)* "Fixed."

> **Interviewer:** "Which code uses that constant?"

> **A:** "...I'm not sure. The export, I think?"

> **Interviewer:** "Do the tests cover this ticket?"

> **A:** "I don't think there's a test for it."

*(In fact the constant was the page size for the notes list in the app. A
now loads 10,000 notes per page for every user, and the export still calls
only page 1.)*

**What the interviewer saw:**

- No reproduction. The fix is a guess.
- No context given to the AI: no code, no files.
- Didn't check where the changed constant is used.
- "Tests pass" was offered as proof, but no test covers the bug.
- Accepted the first answer unchanged.

**Likely scores:** Verification 1, Prompting 1. "Not ready", even if the
guess had happened to work.

---

## Candidate B (strong)

> **B:** "First I want to see how export works." *(searches for `export`,
> opens `routes/export.py` and `services/notes.py`)*

> **B:** "The export calls `NoteService.list_for_user(user_id)`. That
> function takes a `page` argument that defaults to 1, with a page size of
> 100. The export never asks for page 2. That would explain exactly 100."

> **B:** *(to the AI, with both files attached)* "Here are
> `routes/export.py` and `services/notes.py`. Users with more than 100 notes
> get only 100 in the export. I think export only fetches the first page of
> `list_for_user`. Am I right? Is there anything else that could cap it?
> Don't write a fix yet."

> **AI:** "Yes, `export_notes` calls `list_for_user` once, so only page 1 is
> returned. You could also increase the page size to 10,000."

> **B:** "Changing the page size would affect the app's list screen too,
> because `PAGE_SIZE` is shared. I won't do that. I'll write a failing test
> first."

> **B:** *(writes the test)*
> ```python
> def test_export_includes_every_note(client, alice):
>     for i in range(101):
>         create_note(client, alice, f"note {i}")
>     exported = export_notes(client, alice)
>     assert len(exported) == 101
> ```
> "It fails with 100, which proves the bug. For the fix, I'll add a
> repository method that yields all of a user's notes in batches, so a user
> with 50,000 notes doesn't load everything into memory at once."

> **B:** *(after the fix)* "The new test passes, the list endpoint still
> returns 100 per page, and the full suite passes. I also added a test for a
> user with zero notes."

> **Interviewer:** "Anything you didn't check?"

> **B:** "Performance with a really large account. I'd want to try 50,000
> notes and check memory and time. And whether notes created *during* an
> export should be included. That's a product question."

**What the interviewer saw:**

- Read the code path and found the cause before asking for a fix.
- Gave the AI the relevant files and asked for confirmation, not just a
  fix.
- Rejected the AI's weak suggestion (shared page size) with a reason.
- Wrote a failing test first, then confirmed the fix, and checked the
  existing behaviour still worked.
- Named unverified risks honestly.

**Likely scores:** 3–4 across the board.

---

## The difference in one table

| | Candidate A | Candidate B |
|---|---|---|
| First move | Ask AI for a fix | Find and read the code path |
| Context given to AI | None | The two relevant files and a hypothesis |
| What they asked AI for | "Fix this" | "Am I right? What else could it be?" |
| Checked side effects | No (changed a shared constant) | Yes (list page size unchanged) |
| Proof | "Tests pass" | A failing test that then passes |
| AI output | Accepted unchanged | Main point accepted, weak suggestion rejected with a reason |
| Honesty | Claimed it was fixed | Named what wasn't checked |

Candidate B wasn't much slower. Reading the code took two minutes and
avoided a fix that broke something else.
