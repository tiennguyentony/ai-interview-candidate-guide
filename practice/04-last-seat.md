# 4. Last seat

A workshop has limited seats. This function runs on every "Reserve" click.
`db` is an async database client.

```ts
export async function reserveSeat(workshopId: string, userId: string) {
  const workshop = await db.getWorkshop(workshopId);
  if (workshop.seatsTaken >= workshop.capacity) {
    throw new Error('Workshop is full');
  }
  await db.addAttendee(workshopId, userId);
  await db.updateWorkshop(workshopId, { seatsTaken: workshop.seatsTaken + 1 });
}
```

Organisers say: "A workshop with 20 seats ended up with 23 attendees, and
the seat counter says 20."

You asked the AI. It said:

> The capacity check is correct: it throws before adding anyone once
> `seatsTaken` reaches `capacity`. The extra attendees were probably added
> manually by an admin. I'd check the admin logs.

## Your task

1. Explain how 23 people could get in while the counter shows 20, with no
   admin involved. Write out the order of events for two requests.
2. Why does the counter end up lower than the real number of attendees?
3. How could you show this happening in a test, without relying on lucky
   timing?
4. Name two ways to fix it, and one trade-off of each.

**Hint, only if stuck:** two people click "Reserve" for the last seat at the
same moment.
