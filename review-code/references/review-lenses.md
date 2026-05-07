# Review Lenses

Use these heuristics to sharpen each review pass.

## Maintainability

Look for:

- code that is difficult to read or reason about
- duplicated logic that is likely to drift
- abstractions that add indirection without reducing complexity
- shallow wrappers with little value
- oversized functions or modules with mixed responsibilities
- naming that hides intent
- behavior coupled across layers in brittle ways
- tests that are hard to maintain or overly tied to implementation details

High-value maintainability findings usually explain why future changes will be slower, riskier, or more error-prone.

## Efficiency

Look for:

- repeated work inside loops or hot paths
- unnecessary network, disk, or database round trips
- avoidable full scans, repeated parsing, or repeated rendering
- loading or computing more data than needed
- expensive work happening too often
- missing batching, memoization, caching, or early exits when clearly justified

Do not report hypothetical micro-optimizations unless there is a credible impact.

## Project Guidelines

Look for:

- violations of `AGENTS.md`, docs, contribution rules, or architecture guidance
- patterns that conflict with established code in the same area
- incorrect layer boundaries
- missing required tests, validation, or documentation
- new abstractions that bypass the repo's normal design rules

If the repo guidance is unclear, say that instead of inventing a rule.

## Reliability

Look for:

- bugs in happy-path logic
- broken edge cases
- incorrect defaults or assumptions
- missing validation or sanitization
- race conditions and state consistency issues
- partial failure handling gaps
- error mapping problems
- missing cleanup or rollback behavior
- tests that miss important failure modes

Reliability findings should focus on what can break in real usage.
