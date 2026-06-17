---
name: pc-tdd
description: Test-driven development — red-green-refactor loop
---

Follow the TDD cycle for implementing new code.

## Cycle

### Red
Write a failing test first. The test should describe the desired behavior. Run it and confirm it fails.

### Green
Write the minimum code needed to make the test pass. Don't optimise yet. Run the test and confirm it passes.

### Refactor
Improve the code while keeping tests green. Extract duplication, clarify names, simplify logic.

## Rules

- Write the test before the implementation. No exceptions.
- Each cycle should be small — minutes, not hours.
- If you're stuck on the test, the design might need rethinking.
- Run the full test suite after refactor step.
- Delete or rework tests that test implementation details, not behavior.