# 7. Write the context

You've joined a team that builds a small task-tracker API (TypeScript,
Express, PostgreSQL). After exploring for ten minutes, you know this:

- Routes are in `src/routes/`, business logic in `src/services/`, SQL only
  in `src/repositories/`.
- Services throw `NotFoundError`, `ForbiddenError` or `ValidationError` from
  `src/lib/errors.ts`; one middleware turns them into HTTP responses.
- Every task belongs to a project; only project members can see or change
  its tasks. `projectService.requireMember(userId, projectId)` checks this.
- Timestamps are stored in UTC. Code gets the time from an injected `clock`,
  never `new Date()` directly, so tests can control time.
- Tests use Vitest; `npm test` and `npm run typecheck` must pass.
- Migrations are in `migrations/`, numbered. `004_add_labels.sql` is the
  latest and is already deployed.

Your task card says:

> **Due-date reminders.** Users can set a reminder on a task ("remind me 1
> day before" or "1 hour before"). A background job runs every 5 minutes
> and sends a reminder email for any reminder that is due. Each reminder is
> sent once.

## Your task

1. Write the `context.md` you'd give the AI before the first prompt.
   Keep it under 30 lines.
2. Write your plan as 4–6 small steps.
3. List the clarifying questions you'd ask before starting.
4. Write the first prompt you'd send.

There is no single right answer. Compare yours with the example in the
solutions and look for anything important you left out.
