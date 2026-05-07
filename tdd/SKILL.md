---
name: tdd
description: Double-loop test-driven development for behavior-first changes in existing codebases. Use when adding or changing non-trivial behavior and the user wants tests to drive implementation.
---

# TDD (Double-Loop)

Use this skill to drive behavior through small vertical slices.

Follow the repository's existing test stack, naming, helpers, fixtures, and conventions before introducing new patterns.

Do not default to writing tests for everything. Prefer the smallest test surface that protects meaningful behavior.

This workflow is especially aligned with the style from *Growing Object-Oriented Software, Guided by Tests*: drive behavior from the outside, use focused inner-loop tests only when they improve feedback, and let design emerge through small green refactoring cycles.

## Principle

Quality over quantity:

- Test behavior that can break and matters.
- Skip tests for trivial passthrough code, obvious type-only wrappers, and low-value assertions.
- Prefer one strong test over many redundant ones.
- Tests should verify behavior through public interfaces, not implementation details. A good test reads like a specification: it describes what the system does, not how it does it.

## The Double Loop

Two nested feedback loops drive development:

```text
┌─────────────────────────────────────────────────┐
│  Write a failing acceptance test                │
│                                                 │
│    ┌───────────────────────────────────┐        │
│    │  Write a failing unit test    ◄───┐        │
│    │         │                         │        │
│    │         ▼                         │        │
│    │  Make the test pass               │        │
│    │         │                         │        │
│    │         ▼                         │        │
│    │     Refactor ─────────────────────┘        │
│    └───────────────────────────────────┘        │
│                                                 │
│  Acceptance test passes ✓                       │
└─────────────────────────────────────────────────┘
```

Outer loop:

- Defines what done looks like from the outside.
- Exercises the behavior through a public interface.
- Goes green when the feature slice is wired and behaving correctly.

Inner loop:

- Drives logic one behavior at a time.
- Uses focused tests with mocked boundaries when they improve feedback.
- Exists only when the slice has enough logic to justify it.

## Decision Point: Do I Need the Inner Loop?

Before writing a unit test, ask:

- Does this slice have branching, validation, orchestration, or transformation logic? Write an inner-loop test.
- Is it mostly wiring, mapping, or straightforward passthrough? Make the acceptance test pass directly.

The outer loop is the anchor. The inner loop is optional and should be used only when it gives better feedback.

## Anti-Pattern: Horizontal Slices

Do not write all tests first and all implementation later.

This usually creates brittle tests for imagined behavior instead of actual behavior.

Wrong:

- Write several acceptance tests
- Write several unit tests
- Implement everything after that

Right:

1. Write one failing acceptance test for one behavior slice.
2. Add one failing unit test if the implementation logic is non-trivial.
3. Make that slice pass.
4. Refactor while green.
5. Repeat for the next slice.

## Workflow

### 1. Write a Failing Acceptance Test

Start with one test that describes the behavior from the outside.

Examples of public interfaces include:

- a command
- a UI flow
- a module entrypoint
- a job entrypoint
- an event handler
- an application boundary

Use the acceptance test to verify:

- observable outcomes
- caller-visible behavior
- important error behavior
- integration between collaborating parts when that is part of the contract

The acceptance test is the anchor. It is what tells you the slice is actually done.

### 2. Drive Internals with Unit Tests When Needed

If the behavior has meaningful logic, enter the inner loop:

1. Pick one behavior slice.
2. Write one failing test.
3. Implement the minimum code to pass.
4. Refactor without changing behavior.
5. Repeat only for high-value behavior.

These tests should stay close to the module under test and avoid unnecessary integration with slow or unstable dependencies.

## When To Write Unit Tests

Write them when at least one is true:

- Non-trivial branching or decision logic
- Validation or error paths with business impact
- Transformations that are easy to regress
- Orchestration behavior where dependency calls are part of business intent
- Edge cases that are easier to express narrowly

Usually skip them when:

- The code is simple wiring or thin passthrough
- The behavior is already fully protected by the acceptance test
- The test would mostly assert implementation details

### 3. Make the Acceptance Test Green

If the inner loop is green but the acceptance test is still red, the missing piece is usually wiring:

- interface registration or exposure
- input parsing
- error translation
- mapper behavior
- dependency connection

Close that gap until the outer loop passes.

### 4. Refactor

Refactor only while green.

Use both loops as the safety net.

During interface design and refactoring, review:

- [Deep Modules](references/deep-modules.md)
- [Refactor Candidates](references/refactor-candidates.md)

## Mocking Boundaries

Mock true boundaries when needed:

- databases
- network calls
- queues
- file system
- time
- randomness
- auth providers
- payment providers
- external services

Do not mock:

- your own pure logic inside the same behavior slice
- implementation details that are not part of the behavior being verified

Prefer assertions on outcomes over asserting every internal call.

## Repo Adaptation

Before writing tests, inspect the repo and follow existing patterns for:

- test runner
- assertion style
- file naming and test location
- factories and fixtures
- mocking utilities
- setup and teardown
- helper utilities
- verification commands

If the repo already has a testing style, reuse it.

If the repo has no clear pattern, introduce the smallest consistent approach that fits the stack.

## Checklist Per Cycle

- Test describes behavior, not implementation.
- Test uses a public interface for the layer under test.
- Code is minimal for this slice.
- No speculative features were added.
- Refactoring happens only while green.

## Guardrails

- Do not implement multiple slices at once.
- Do not let unit tests replace the acceptance test.
- Do not add low-value tests for trivial code.
- Do not refactor while red.
- Do not optimize coverage numbers over behavior protection.

## Final Check

- Does the acceptance test prove the behavior that matters?
- Does each unit test help close a real gap?
- Is any test redundant or brittle?
- Would the tests survive internal refactors?
- Is the suite small but sufficient?
