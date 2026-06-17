---
name: pc-diagnose
description: Debugging loop — reproduce, minimise, hypothesise, fix
argument-hint: [describe the problem]
---

Follow this structured debugging loop when investigating a bug or failure.

## Steps

### 1. Reproduce
Get a reliable, repeatable reproduction. Document exact steps, inputs, environment.

### 2. Minimise
Strip away everything that isn't relevant to the failure. Reduce the reproduction to the minimum code/config/input that still triggers it.

### 3. Hypothesise
State one or more hypotheses for the root cause. For each, predict what you'd expect to see if it's correct. Test the most likely first.

### 4. Fix
Once root cause is confirmed, implement the fix. Verify the fix against the reproduction case. Run existing tests to confirm no regressions.

## Rules

- Never skip step 2 — most bugs become obvious once isolated
- If a hypothesis fails, document why and move to the next
- Commit the reproduction case if it's testable (regression test)
- If stuck after 3 failed hypotheses, zoom out and reconsider the problem framing