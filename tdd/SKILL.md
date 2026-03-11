---
name: tdd
description: Test-driven development workflow using Vitest for unit and acceptance tests. Use when adding or changing non-trivial behavior and the user wants test-first or behavior-first development.
---

# TDD (Vitest)

## Principle

Quality over quantity:

- Test behavior that can break and matters.
- Skip tests for trivial passthrough code, obvious type-only wrappers, and low-value assertions.
- Prefer one strong test over many redundant ones.

## When To Write Tests

### Unit Tests

Write unit tests when at least one is true:

- Non-trivial branching/decision logic
- Validation/error paths with business impact
- Transformations that are easy to regress
- Orchestration behavior (which dependency is called, with what payload)

Skip when:

- Simple CRUD/passthrough with no branching
- Behavior already fully covered by acceptance tests
- Test would only assert implementation details

### Acceptance Tests

Write acceptance tests when at least one is true:

- End-to-end flow through HTTP endpoints matters for correctness
- Authorization/permissions logic needs verification
- Multiple components must integrate correctly (route → use case → repository → database)
- Data persistence through the full stack needs validation

Skip when:

- The endpoint is a thin wrapper with no business logic
- The behavior is already fully covered by unit tests and the integration risk is low

## Unit-Test TDD Loop

1. Pick one behavior slice.
2. Write one failing unit test.
3. Implement the minimum code to pass.
4. Refactor without changing behavior.
5. Repeat only for high-value behaviors.

### Unit Test Guidelines

- Keep tests next to the source (`*.unit.test.ts` or `__tests__/`).
- Mock repositories and external systems; avoid DB/network in unit tests.
- Use `vi.mock(...)` for external dependencies.
- Focus on behavior groups with `describe`.
- Validate call payloads and transaction usage.
- Cover validation failures and key branch outcomes.
- Prefer behavior assertions over internal function spying.

## Acceptance-Test TDD Loop

1. Define the user-facing behavior (HTTP request → expected response + side effects).
2. Write one failing acceptance test covering the happy path.
3. Implement route, use case, and repository code to pass.
4. Add tests for error paths, authorization, and edge cases.
5. Refactor internals; acceptance tests should survive refactors.

### Acceptance Test Guidelines

- Test the complete HTTP request/response cycle.
- Use a real database; set up and tear down test data per test.
- Use helper functions for test data creation (avoid raw DB inserts in test cases).
- Verify authorization, permissions, and correct error codes.
- Test cross-entity isolation (data from one tenant doesn't leak to another).
- Clean up test data in `afterEach` to prevent test pollution.

## PR Checklist

- Does each test protect meaningful behavior?
- Is any test redundant or brittle?
- Would tests survive internal refactors?
- Is the suite small but sufficient for regression safety?
- Do acceptance tests cover authorization and error responses?
