# 2. The broken streak

A habit-tracking app counts how many days in a row you've logged a workout.
Each log is filed under a "day key". This runs in the user's browser.

```ts
export function dayKey(loggedAt: Date): string {
  return loggedAt.toISOString().slice(0, 10); // 'YYYY-MM-DD'
}
```

A user in New York writes:

> I always work out in the evening. Last night I logged at 11 pm, and the
> app filed it under **tomorrow**. Today's workout then counted for the same
> day and my 40-day streak reset to 1!

The AI's reply when you pasted the report and the code:

> `toISOString()` returns a standard date format, so the day key is correct.
> The user's device clock is probably wrong. Ask them to enable automatic
> time.

## Your task

1. Who is right? Explain exactly what happens at 11 pm in New York.
2. How would you prove it? (A command, a test or a short script.)
3. Which users does this affect, and at what times of day? Would a
   developer in London notice it?
4. How would you fix it? What does "a day" mean for this app, and who
   should decide?

**Hint, only if stuck:** which time zone does `toISOString()` use?
