# 1. Old enough?

A ticket-selling site must refuse sales of 18+ events to anyone under 18.

```python
from datetime import date

def age_on(birth_date: date, today: date) -> int:
    """Age in whole years on the given day."""
    return (today - birth_date).days // 365


def can_buy_adult_ticket(birth_date: date, today: date) -> bool:
    return age_on(birth_date, today) >= 18
```

You asked an AI to review it. It answered:

> This is correct. Subtracting two dates gives a `timedelta`, and dividing
> the days by 365 gives the age in years. Taking `today` as a parameter is
> good for testing. Ready to merge.

## Your task

1. Is the AI right? Find a concrete birth date and `today` where the answer
   is wrong, and say which way it's wrong (too young or too old).
2. Why is this particular direction of error worse for this business?
3. Write the test cases you'd add (inputs and expected outputs).
4. Sketch a correct `age_on`.
5. Is there a case where even the *correct* answer needs a product decision?

**Hint, only if stuck:** try someone whose 18th birthday is tomorrow.
