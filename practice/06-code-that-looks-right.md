# 6. Code that looks right

You asked an AI: *"Fetch all orders, call the shipping API for each one in
parallel, then group the successful results by status."* It wrote this
TypeScript for Node.js:

```ts
export async function shippingReport(orderIds: string[]) {
  const results = await Promise.allSettled(
    orderIds.map((id) => shippingApi.track(id)),
  );

  const ok = results.filter((r) => r.ok).map((r) => r.value);

  return ok.groupBy((shipment) => shipment.status);
}
```

It added: *"This uses modern JavaScript and handles failures gracefully
thanks to `Promise.allSettled`."*

## Your task

1. Find everything that doesn't work as the AI claims. Be specific about
   what happens at runtime.
2. For each problem, how would you have caught it quickly without the AI?
   (Docs? The type checker? Running it?)
3. Write a corrected version.
4. Is there a behaviour question you'd raise about the original request?

**Hint, only if stuck:** check the documentation for what
`Promise.allSettled` returns, and whether arrays have a `groupBy` method.
