# Testing Fundamentals

Quick reference for building a testing strategy for your project.

## The Testing Pyramid

```
        /  E2E  \          ← Few, slow, expensive
       / Integration \     ← Moderate number
      /   Unit Tests  \    ← Many, fast, cheap
```

- **Unit Tests** — test a single function or class in isolation. Fast and cheap. This is your foundation.
- **Integration Tests** — test how modules work together (e.g., API → Database). Slower but catches wiring bugs.
- **End-to-End (E2E) Tests** — test the full system from the user's perspective. Slowest but most realistic.

**Rule of thumb:** 70% unit, 20% integration, 10% E2E.

## Test Types at a Glance

| Type | What it checks | Speed | When to use |
|---|---|---|---|
| Unit | Single function/class logic | Fast (<1ms) | Always, for all business logic |
| Integration | Module interactions, DB/API connections | Medium | For data flows and external calls |
| E2E | Full user workflow | Slow | For critical user paths only |
| Performance | Response time under load | Varies | Before launch, for high-traffic features |
| Load | System behavior under expected traffic | Slow | Before launch |
| Stress | System behavior beyond expected load | Slow | To find breaking points |

## What to Test

**Always test:**
- Happy path (normal input → expected output)
- Edge cases (empty input, boundary values, null)
- Error cases (invalid input → proper error response)

**Don't test:**
- Framework/library internals (trust the library)
- Simple getters/setters with no logic

## Mocking — When and What

**Mock** external dependencies when:
- The real thing is slow (database, network)
- The real thing has side effects (sending emails, writing files)
- You need to test error scenarios that are hard to reproduce

**Mock these:**
- Database calls
- External API calls
- File system operations
- Time-dependent logic

**Don't mock these:**
- Your own business logic (test it directly)
- Data transformations (test with real data)
- Simple utility functions

## Test Naming Convention

Name your tests so they read like documentation:

```
test_{method}_{scenario}_{expectedResult}

Examples:
test_createUser_validInput_returns201
test_createUser_duplicateEmail_returns409
test_getUser_notFound_returns404
test_calculateDiscount_expiredCoupon_returnsZero
```

## Coverage

Aim for **70%+ coverage** on:
- Branches (if/else paths)
- Functions
- Lines
- Statements

But remember: **high coverage ≠ good tests**. A test that asserts nothing can still count as "covered."

Focus on testing the right things, not hitting 100%.

## Best Practices

- Each test must be **independent** — no shared state between tests
- Tests must be **deterministic** — same input always gives same result
- Run tests on every commit (CI/CD)
- If a test fails, fix it immediately — don't ignore flaky tests
- Write tests alongside code, not after

## Common Testing Frameworks

| Language | Framework |
|---|---|
| JavaScript/TypeScript | Jest, Vitest |
| Python | pytest |
| Java | JUnit 5 |
| Go | testing (stdlib) |
| C# | xUnit, NUnit |

---

Adapted from [java-tech-interviews-prep](https://github.com/marttp/java-tech-interviews-prep)
