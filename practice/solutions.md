# Practice solutions

Try each exercise before reading its solution. For each one, the key
question is not just "did I find it?" but "did I **prove** it?"

---

## 1. Old enough?

**The AI is wrong.** Dividing days by 365 ignores leap days. Every four years
adds a day, so the formula counts people as a year older a few days before
their real birthday.

- Born 2008-10-12, checked on 2026-10-11 (the day **before** their 18th
  birthday): `(today - birth).days` is 6573, and `6573 // 365` is **18**.
  The function says 18; the person is 17.
- The error is always in the same direction: **too old**. Around each
  birthday there's a window of a few days (it grows with age) where a
  17-year-old passes the check.

**Why that direction is worse:** saying "too young" annoys a customer.
Saying "too old" sells an 18+ ticket to a minor, which is a legal problem.

**Proof:** one test:
`assert can_buy_adult_ticket(date(2008, 10, 12), date(2026, 10, 11)) is False`.
It fails today.

**Tests worth having:** the day before, the day of, and the day after the
18th birthday; someone born on 29 February; a very old customer; a birth
date in the future (invalid input).

**A correct version** compares calendar fields, not day counts:

```python
def age_on(birth_date: date, today: date) -> int:
    had_birthday = (today.month, today.day) >= (birth_date.month, birth_date.day)
    return today.year - birth_date.year - (0 if had_birthday else 1)
```

**Product decision:** someone born on 29 February. In a non-leap year, do
they turn 18 on 28 February or on 1 March? The correct code above says
1 March. Different countries' laws differ, so ask; don't let the AI decide.

**Lesson:** "it uses the standard library" doesn't make date arithmetic
right. Test the boundaries.

---

## 2. The broken streak

**The user is right.** `toISOString()` always formats the time in **UTC**.
At 11 pm in New York during daylight saving time (UTC−4), it is already
3 am the next day in UTC. So an 11 pm workout gets the next day's key, and
the next evening's workout lands on that same key. One day looks empty and
the streak resets.

**Proof:** in Node,

```js
new Date('2026-10-12T03:00:00Z').toISOString().slice(0, 10)
// '2026-10-12', but in New York that moment is 11:00 PM on 2026-10-11
```

A test with a fixed time (not "now") shows it. Notice the function is easy
to test because it takes the time as a parameter.

**Who's affected:** anyone west of UTC who logs in the evening (the
Americas), and anyone east of UTC who logs after midnight local time but
before midnight UTC (much of Asia, early morning). A developer in London
would only see it in summer, and only for an hour around midnight, so it
can easily slip through.

**A fix:** compute the key in the user's time zone:

```ts
export function dayKey(loggedAt: Date, timeZone: string): string {
  return loggedAt.toLocaleDateString('en-CA', { timeZone }); // 'YYYY-MM-DD'
}
```

**Product questions:** which time zone defines "a day": the device's
current zone, or one saved in the user's profile? What happens when the
user travels? Some apps also let a day end at 3 am for night owls. These are
decisions for the product owner.

**Lesson:** "the user's clock is wrong" is a classic AI deflection.
`toISOString()`, date-only strings and "today" are among the most common
sources of real bugs.

---

## 3. The test that agrees with everything

**The parametrized test can't fail.** `expected` is computed with the
**same formula** as the function. If the formula were wrong, the test would
compute the same wrong answer. It mirrors the code instead of checking it.
`test_returns_a_string` proves almost nothing.

**Better tests** state the expected output by hand, from what users should
see:

```python
@pytest.mark.parametrize("seconds, expected", [
    (0, "0h 00m"),
    (60, "0h 01m"),
    (3599, "0h 59m"),
    (3600, "1h 00m"),
    (3900, "1h 05m"),
    (90000, "25h 00m"),
])
def test_format_duration(seconds, expected):
    assert format_duration(seconds) == expected
```

**Behaviour worth questioning** (run it to see):

- `format_duration(-60)` returns **`"-1h 59m"`**, because `//` rounds down
  for negative numbers. Should negative input be an error?
- `format_duration(59)` returns `"0h 00m"`. Should a 59-second video show
  "0h 01m", or seconds?
- `format_duration(90000)` returns `"25h 00m"`. Is that what the product
  wants, or "1d 1h"?

Those are questions for the product owner. The mirrored tests would never
have raised them.

**How to check a test is meaningful:** break the function on purpose (for
example change `// 60` to `// 30`) and run the tests. The original tests
still pass. Yours fail. That's the difference.

---

## 4. Last seat

**The AI is wrong; this is a race condition (check-then-act, lost update).**

Two requests, A and B, for the last seat (capacity 20, `seatsTaken` 19):

1. A reads the workshop: `seatsTaken = 19`.
2. B reads the workshop: `seatsTaken = 19`.
3. A: 19 < 20, so it adds A's attendee.
4. B: 19 < 20, so it adds B's attendee.
5. A writes `seatsTaken = 20`.
6. B writes `seatsTaken = 20`.

Result: 21 attendees, counter 20. With four requests reading 19 at the same
time you get 23 attendees and the counter still says 20. The counter is low
because each request writes "what I read + 1", overwriting the others'
increments.

**Proving it without luck:** make the interleaving happen deterministically.
For example, use a fake `db` whose `getWorkshop` waits on a promise you
control, start two `reserveSeat` calls, let both reads finish, then release
them. Assert attendees = 21. (Or `Promise.all` on many calls against an
in-memory fake that yields between steps.)

**Fixes and trade-offs:**

- **Atomic conditional update in the database:**
  `UPDATE workshops SET seats_taken = seats_taken + 1 WHERE id = $1 AND
  seats_taken < capacity` and check that one row changed. Simple and fast,
  but the logic moves into SQL.
- **Transaction with a row lock** (`SELECT ... FOR UPDATE`) around the
  check and both writes. Clear, but requests queue on hot workshops.
- **Constraint-based:** count attendees with a database constraint or
  trigger instead of a separate counter. Can't drift, but more complex.
- Also: add attendee and update counter should be **one transaction**, or
  they can disagree after a crash.

**Lesson:** "the check is correct" is true for one request at a time. Always
ask "what if two of these run at once?"

---

## 5. "This is a bug"

**It's very likely not a bug.** Before changing it, a strong candidate
checks:

- the decision record `007-password-reset-responses.md`,
- the test, which **pins** the same response for unknown emails on purpose,
- `git log` / blame for why it was written this way.

**What it protects against:** if unknown emails got a 404, anyone could
type in email addresses and learn who has an account (account
enumeration). That helps attackers with phishing and password guessing,
and can reveal private facts (for example, that someone uses a particular
health or dating app). Returning the same message either way hides that.

**The warning sign:** the AI changed the test so its change would pass. When
a "fix" requires editing a test that encodes intended behaviour, stop and
find out why the test says what it says.

**Helping users who mistype:** show the email back ("We've sent a link to
j\*\*\*@example.com if an account exists"), suggest checking spam, offer
a "resend" option, and validate the email format before submitting. None of
these reveal whether the account exists.

**Lesson:** AI tools often report deliberate security design as a bug,
especially around error messages. Look for the reason before "fixing".

---

## 6. Code that looks right

**Problems:**

1. **`r.ok` doesn't exist.** `Promise.allSettled` returns objects shaped like
   `{ status: 'fulfilled', value }` or `{ status: 'rejected', reason }`.
   `r.ok` is `undefined`, so the filter silently removes **everything**.
   No error, just an empty result.
2. **Arrays have no `groupBy` method.** The proposal ended up as
   `Object.groupBy(items, fn)` (and `Map.groupBy`). `ok.groupBy(...)`
   throws `TypeError: ok.groupBy is not a function` at runtime.
3. Failures are dropped without a trace. Nobody learns which orders failed.
4. It starts one request per order with no limit. With 5,000 orders that
   may hit the shipping API's rate limit.

**How to catch them fast:**

- The **TypeScript type checker** flags both: `Property 'ok' does not exist`
  and `Property 'groupBy' does not exist on type ...[]`.
- **Run it** once with two fake ids: you get a `TypeError`.
- **Read the docs** for `Promise.allSettled` and `Object.groupBy`.

**Corrected:**

```ts
export async function shippingReport(orderIds: string[]) {
  const results = await Promise.allSettled(orderIds.map((id) => shippingApi.track(id)));

  const shipments = results.flatMap((r) => (r.status === 'fulfilled' ? [r.value] : []));
  const failed = orderIds.filter((_, i) => results[i].status === 'rejected');

  return { byStatus: Object.groupBy(shipments, (s) => s.status), failed };
}
```

(`Object.groupBy` needs Node 21+ and `"lib": ["ES2024"]` or later in
`tsconfig.json`.)

**Behaviour questions to raise:** should failures be reported, retried or
ignored? Is there a concurrency limit for the shipping API?

**Lesson:** plausible-looking code that uses an API "almost right" is one of
the most common AI failures. The type checker and a single run catch most of
it in seconds.

---

## 7. Write the context

One good example (yours can differ):

```markdown
## What I'm building
Task reminders: a user sets "1 day before" or "1 hour before" a task's due
date; a job every 5 min emails due reminders, each exactly once.

## Rules
- Layers: routes → services → repositories. SQL only in repositories.
- Errors: throw NotFoundError / ForbiddenError / ValidationError
  (src/lib/errors.ts). Never send HTTP responses from services.
- Permissions: call projectService.requireMember(userId, projectId) before
  reading or changing anything on a task.
- Time: use the injected clock, never new Date(). Store UTC.
- Migrations: add 005_*.sql. Never edit 001–004 (already deployed).

## Definition of done
- Set / list / delete reminder endpoints, members only.
- Job sends each due reminder once, even if it runs twice at the same
  time or crashes after sending.
- Tests with a fake clock: due / not yet due / already sent / non-member /
  task due date changed after the reminder was set.
- npm test and npm run typecheck pass. No unrelated changes.

## Out of scope
- Push notifications, custom reminder times.
```

**Plan:**

1. Migration `005_task_reminders.sql`: table with task, user, offset,
   `remind_at`, `sent_at`.
2. Repository + service to set / list / delete, with `requireMember` and
   validation. Unit tests.
3. Routes. Integration tests including a non-member.
4. Job: claim due reminders atomically (e.g. `UPDATE ... SET sent_at = now
   WHERE sent_at IS NULL AND remind_at <= now RETURNING ...`), then send.
   Tests with the fake clock.
5. Edge cases: task due date changed or task deleted; reminder already in
   the past when set.

**Clarifying questions:**

- If the task's due date changes, should the reminder move with it?
- What if the reminder time is already in the past when it's set?
- "Sent once": is it OK to miss one if the email provider is down
  (at-most-once), or must it retry and risk a duplicate (at-least-once)?
- Who receives it: the person who set it, or the task's assignee?
- Can a user set several reminders on one task?

**First prompt** (example):

> Here is context.md and the files src/services/taskService.ts,
> src/repositories/taskRepo.ts and migrations/004_add_labels.sql. Propose
> the schema for migrations/005 and the service method signatures for
> setting, listing and deleting reminders. Don't write code yet. Point out
> anything in my plan that conflicts with the rules.

**What people often miss:** permissions on the new endpoints, the
"exactly once" problem when two job runs overlap, the injected clock, and
editing an old migration instead of adding a new one.

---

## 8. Review the AI's pull request

**Review comments:**

- `routes/projects.ts`, new route: **no permission check.** Anyone logged in
  can archive any project, though the description says "only owners".
- Same route: **no 404.** If the project doesn't exist, `project` is
  `undefined` and `project.id` throws a 500. Should throw `NotFoundError`.
- Same route: **calls the repository directly** from the route, skipping
  the service layer. Logic belongs in `projectService.archive(...)`.
- Same route: **`new Date()`** instead of the injected clock, so it can't be
  tested deterministically.
- **Missing migration.** The code uses `archived_at` / `archivedAt`, but no
  migration adds that column. Unless it already exists, this fails in
  production.
- `projectRepo.listForUser`: fine, but should archived projects still be
  reachable by id, and can they be unarchived? (Question for the author.)
- `config.ts`: **unrelated change.** The request timeout went from 5 s to
  30 s with no explanation. This contradicts "no other behaviour changes"
  and could hide slow-request problems. Revert or explain.
- `tests/projects.test.ts`: only checks the status code for the happy path.
  Missing: a non-owner gets 403/404, a missing project gets 404, the
  archived project disappears from the list, other members' lists are
  affected correctly.

**False or unproven claims:** "only owners can archive" (false), "fully
tested, including edge cases" (false), "no other behaviour changes" (false,
the timeout), "all tests pass" (unproven, and the missing migration may
mean they don't in a clean environment).

**Decision:** request changes. Minimum to approve: permission check, 404
handling, service layer with injected clock, a migration if needed, the
config change reverted or justified, and tests for the cases above.

**Questions for the teammate:** "Did you read the diff before opening the
PR?", "Why did the timeout change?", "Did you run the tests locally on a
fresh database?" Ask kindly. The goal is a team habit, not blame.

**Lesson:** compare the description with the diff line by line. AI-written
descriptions often claim more than the code does.
