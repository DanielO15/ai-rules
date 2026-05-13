---
name: gen-tests
description: Generate unit tests for a function or file. Use when the user runs `/gen-tests <function or file>` or asks to "write tests for X", "add tests for this", "cover this with tests".
user-invocable: true
---

Generate tests for the function or file specified in `$ARGUMENTS`. If no argument is given, use the most recently discussed or edited function in this conversation.

## Step 1: Read the target

Read the file containing the function. Identify:
- The function signature and what it returns
- Its dependencies (what it imports or calls)
- Any existing tests for it — check for a `*.test.*` or `*.spec.*` file alongside it, or in a `__tests__` directory

## Step 2: Read existing test patterns

Find 1-2 existing test files in the codebase to understand:
- The test framework in use (Jest, Vitest, pytest, etc.)
- How mocks and fixtures are set up
- File naming and structure conventions

Follow these patterns exactly — do not introduce a new style.

## Step 3: Write the tests

Write tests covering:

1. **Happy path** — the expected inputs produce the expected output
2. **Error cases** — what happens when an API call fails, a value is null, input is malformed
3. **Edge cases** — boundary values, empty arrays, zero, etc. — only include ones that could realistically occur

For each test, make sure:
- The test name describes the behaviour, not the implementation ("returns empty array when no users found" not "test getUsersEmpty")
- Mocks are scoped to the test, not global where avoidable
- You're testing behaviour, not implementation details

## Step 4: Show and explain

Show the tests. Then briefly explain:
- What each group covers and why
- Any case you deliberately left out and why

This is the learning part — understanding what makes a good test suite for this specific function, not just having coverage.

## Important rules

- Do NOT change the function being tested
- If the function is untestable as written (e.g. too many side effects, no dependency injection), flag it and suggest the minimal change needed to make it testable — but do not make that change without confirmation
- Match the existing test framework — do not introduce new dependencies
