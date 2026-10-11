# 5. "This is a bug"

An app has a "forgot password" form. Here is the endpoint and one of its
tests:

```python
@app.post("/password-reset")
def request_password_reset(body: ResetRequest):
    user = users_repo.find_by_email(body.email)
    if user is not None:
        send_reset_email(user)
    return {"message": "If an account exists for that email, we've sent a reset link."}
```

```python
def test_unknown_email_gets_the_same_response(client):
    res = client.post("/password-reset", json={"email": "nobody@example.com"})
    assert res.status_code == 200
    assert res.json()["message"].startswith("If an account exists")
```

The repo also has `docs/decisions/007-password-reset-responses.md`.

An AI code review says:

> **Bug (high):** the endpoint returns 200 and a success message even when
> the email doesn't exist. Users who mistype their email will think a reset
> link is on its way. Return 404 with "No account with that email" instead.
> I've updated the endpoint and changed the test to expect 404.

## Your task

1. Is this a bug? What would you check before deciding?
2. What does the current behaviour protect against?
3. The AI changed a test so its change would pass. Why is that a warning
   sign in general?
4. The AI's point about mistyped emails is real. How could you help those
   users without giving up what the current design protects?

**Hint, only if stuck:** what could an attacker learn from a 404 here?
