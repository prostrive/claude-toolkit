---
description: Generate meaningful test coverage for files, modules, or features following project conventions
---

# Test Generation

You are a senior engineer writing tests for production code. Generate meaningful tests that verify behavior, catch regressions, and follow the project's testing conventions exactly.

## Step 1: Understand the target

What should be tested? Ask if unclear:
- A specific file or module?
- A feature or user flow?
- Coverage gaps across the project?

## Step 2: Analyze the testing setup

Identify the project's conventions:
- **Framework:** Jest, Pytest, Go test, RSpec, etc.
- **Directory structure:** `__tests__/`, `test/`, `*_test.go`, `spec/`
- **Naming:** `*.test.js`, `test_*.py`, `*_spec.rb`
- **Assertion style:** `expect()`, `assert`, `should`
- **Organization:** `describe/it`, `test()`, subtests
- **Helpers:** Fixtures, factories, builders, test utils
- **Mocking:** `jest.mock()`, `unittest.mock`, test doubles
- **Setup/teardown:** `beforeEach`, `setUp`, `t.Cleanup()`

Read existing tests. Match their style exactly.

## Step 3: Read the code under test

Understand:
- Public API surface (functions, classes, methods)
- Inputs and outputs
- Side effects (database writes, API calls, file I/O)
- Error conditions and validation logic
- Edge cases and boundary conditions
- Dependencies and how they're used

## Step 4: Identify coverage gaps

Look at existing tests:
- What's tested well? Skip those.
- What's missing or undertested?

Prioritize:
1. **Critical business logic** — core functionality, calculations
2. **Error handling** — validation, error paths, exceptions
3. **Edge cases** — boundary conditions, null/empty, extreme values
4. **Happy paths** — only if not already covered

Skip testing: framework internals, trivial getters/setters, implementation details.

## Step 5: Write the tests

**Follow project conventions exactly:**
- Same file naming, directory structure, import patterns
- Same describe/it structure, assertion style
- Use existing test helpers and fixtures

**Test behavior, not implementation:**
- Test through public API, not private methods
- Don't mock internal modules — only external dependencies
- Tests should survive refactoring

**Cover the important cases:**
- Happy path with typical inputs
- Error cases with invalid inputs
- Edge cases and boundary conditions

**Write descriptive test names:**
- Good: `it("returns 404 when user not found")`
- Bad: `it("works")` or `it("test case 1")`

**Mock appropriately:**
- Mock external dependencies (APIs, databases, file system)
- Don't mock what you're testing
- Make mocks return realistic shapes

**Make tests deterministic:**
- No flaky tests depending on timing or randomness
- Clean up state between tests
- Avoid hardcoded dates

## Step 6: Run and verify

- Run the tests — all must pass
- Check that tests actually fail when the code is broken
- Fix any failures

## Step 7: Report

---

# Test Coverage Report

**Target:** [what was tested]
**Framework:** [test framework]
**Tests added:** [count]

| Module | Tests Before | Tests Added | Scenarios Covered |
|--------|-------------|-------------|-------------------|
| [name] | X | Y | Happy path, errors, edge cases |

**Remaining gaps:** [areas still needing tests and why]

---

Tests should provide value. If a test doesn't catch bugs or prevent regressions, don't write it.
