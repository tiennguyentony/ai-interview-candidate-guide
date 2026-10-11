# Concepts AI tools often get wrong

These topics come up again and again in real bugs, and AI tools are
especially likely to miss them or explain them confidently but wrongly. For
each: what it is, what the bug looks like, and how to test for it.

You don't need to be an expert. You need to recognise when code touches one
of these areas and slow down.

---

## Time zones and dates

**What:** an instant (a moment in time) is not the same as a calendar date
or a wall-clock time. "October 11" starts at different instants in Tokyo,
London and New York. Daylight saving time means a day can have 23 or 25
hours.

**Bugs look like:** things happening an hour early or late after a clock
change; evening events filed under the next day; date-only strings
parsed as midnight UTC; comparing a local time with a UTC time.

**Test:** fixed "now" values near midnight, in a non-UTC time zone, and
across a daylight-saving change. Code that calls the system clock directly
is hard to test; look for an injected clock.

## Money and rounding

**What:** floating-point numbers can't represent most decimal amounts
exactly (`0.1 + 0.2` is `0.30000000000000004`). Rounding rules differ:
Python's `round()` rounds halves to the nearest even number.

**Bugs look like:** totals off by a cent; split amounts that don't add up
to the whole; tax computed per line and per order giving different totals;
interest calculated in floats.

**Test:** amounts that don't divide evenly (100 / 3), half-cent cases, and
the property "the parts add up to the total". Prefer integer cents or a
decimal type with an explicit rounding rule.

## Race conditions and lost updates

**What:** two requests running at the same time interleave. "Read a value,
decide, write a new value" is unsafe if someone else writes in between.

**Bugs look like:** overselling the last item; counters lower than they
should be; duplicate records.

**Test:** run two calls with a forced interleaving (a fake that pauses
between read and write), or many concurrent calls. Fixes: atomic updates
(`SET x = x + 1 WHERE ...`), database constraints, transactions with locks.

## Transactions

**What:** several writes that must all succeed or all fail together.

**Bugs look like:** an order created without its items; money taken but no
booking; inconsistent data after a crash or an error halfway through.

**Test:** make a later step fail (inject an error) and check nothing from
the earlier steps was saved.

## Idempotency and retries

**What:** clients and networks retry. An idempotent operation gives the same
result if repeated. APIs often accept an idempotency key so a retried
request doesn't act twice.

**Bugs look like:** double charges; duplicate orders after a timeout;
"exactly once" claims that are really "at least once".

**Test:** send the same request twice (same key) and check the effect
happened once and the second response matches the first.

## Authentication vs authorization

**What:** authentication is *who you are*; authorization is *what you're
allowed to do*. A logged-in user should only reach their own data.

**Bugs look like:** changing an id in a URL shows someone else's record
(IDOR); trusting a value the client controls; a new endpoint that forgot
the permission check other endpoints have.

**Test:** two users; user A requests user B's resource by id. Also try
requests with no login and with forged headers.

## Injection

**What:** user input treated as code: SQL built with string formatting,
shell commands built from input, HTML inserted without escaping (XSS),
spreadsheet formulas in CSV exports.

**Bugs look like:** a query that returns more than it should when given a
quote character; a script running inside a page; a filename that runs a
command; a spreadsheet cell that runs a formula.

**Test:** put these strings into every input that reaches a query, a
command, a page or an export. Fixes: parameterised queries, argument arrays
instead of shell strings, escaping output, prefixing risky CSV cells.

## Off-by-one and pagination

**What:** boundaries: `<` vs `<=`, 0-based vs 1-based pages, inclusive vs
exclusive ranges.

**Bugs look like:** the last item missing; items duplicated or skipped
across pages; a value exactly on a threshold falling on the wrong side.

**Test:** exact boundary values, first and last page, empty results, and
several items with identical timestamps.

## Caching

**What:** keeping a copy of data to answer faster. The hard part is knowing
when the copy is stale.

**Bugs look like:** stale data shown after a change; one server instance
seeing old data; caches that grow forever.

**Test:** read (to fill the cache), change the data, read again
immediately.

## Error handling

**What:** what happens when something fails.

**Bugs look like:** `except: pass` or `catch {}` hiding failures; returning
success when the operation failed; unhandled promise rejections crashing a
Node.js process; errors that leak internal details or secrets.

**Test:** make dependencies fail (network, database, bad input) and check
the response, the logs and that the process stays up.

## Secrets and logging

**What:** tokens, passwords, API keys and personal data must not end up in
logs, URLs, error messages or AI chats.

**Bugs look like:** secrets or personal data appearing in logs, URLs that
get logged, or error responses.

**Test:** trigger the code path and search the captured logs for the secret.

## Performance basics

**What:** code that is correct but slow at real sizes.

**Bugs look like:** one database query per item in a loop (N+1); reading a
whole large file into memory; regular expressions that take forever on some
inputs (ReDoS).

**Test:** count queries in a test; use a large input; time a pathological
string.

---

## Design decisions that look like bugs

Not everything unusual is wrong, and AI tools love to "fix" deliberate
choices. Common reasons code looks odd on purpose:

- **Security:** hiding information an attacker could use, such as whether
  an account or record exists.
- **Data integrity:** enforcing a rule in the database instead of (or as
  well as) in application code.
- **History:** keeping the values that applied at the time of an event
  rather than recomputing them from today's data.
- **Honest guarantees:** designing for "at least once" with safe retries
  rather than promising something the system can't deliver.
- **Protecting the system:** limits, timeouts and cut-offs that drop work
  on purpose instead of letting one user slow everyone down.

When something looks odd, search for a reason (design docs, ADRs, comments,
tests that pin the behaviour, `git log`) before changing it. Practice
exercise 5 in [practice/](practice/README.md) is an example.
