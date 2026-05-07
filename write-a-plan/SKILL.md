---
name: write-a-plan
description: Create a repo-native implementation plan for one coherent change before coding. Use when the user wants a plan, execution breakdown, implementation roadmap, or a written handoff another engineer or agent can execute.
---

# Write a Plan

Announce at start: "I'm using the write-a-plan skill to create the implementation plan."

## Goal

Create an implementation plan that another engineer or agent can execute with low ambiguity, assuming they only have the repo, the plan, and the repo's local guidance.

The plan must be grounded in the actual codebase, follow project rules, and stop before implementation.

## Workflow

1. Clarify the request.
   - Identify the user goal, scope, success criteria, and constraints.
   - If the request spans multiple independent subsystems, recommend separate plans.
   - If the request is too large to ship coherently as one slice, stop and recommend splitting it into multiple implementation plans instead of writing one oversized plan.

2. Read repo guidance first.
   - Read `AGENTS.md` and any relevant README, docs, contribution guidance, architecture notes, or testing rules.
   - Preserve current project conventions and priorities instead of inventing new ones.

3. Explore the active implementation.
   - Verify current behavior in code before planning changes.
   - Prefer active runtime paths over legacy or dead code.
   - Identify the routes, pages, commands, modules, services, use-cases, repositories, contracts, tests, and docs that matter.

4. Resolve open questions before writing the plan.
   - If a question can be answered by exploring the repo, explore the repo instead of asking the user.
   - If the answer has product, UX, or architectural consequences that cannot be discovered locally, ask the user before writing the plan.
   - Do not leave material execution blockers unresolved.

5. Define the implementation shape.
   - Shape the work as one self-contained, shippable implementation slice.
   - If the work naturally breaks into multiple shippable slices, recommend separate plans instead of a monster plan.
   - Prefer dependency-ordered tasks.
   - Name exact file paths when they can be identified confidently.

6. Write the plan.
   - If the repo already has a planning or docs location, reuse it.
   - If the user asks to save the plan and the repo has no established location, use `docs/plans/YYYY-MM-DD-short-slug.md`.
   - If the user does not ask for a saved file, provide the plan inline unless the repo clearly expects plan documents in-repo.
   - Make the plan readable by someone who has not seen the discussion.

7. Self-review the plan.
   - Check coverage against the original request.
   - Remove placeholders and vague wording.
   - Verify file paths, architectural assumptions, and test strategy against the codebase.

8. Stop after planning.
   - Do not implement code changes unless the user explicitly asks to proceed.

## Plan Format

Every plan should use this structure:

```md
---
title: <short title>
status: draft
created: YYYY-MM-DD
updated: YYYY-MM-DD
---

# <Title>

## Goal

A short statement of what this change should accomplish.

## Success Criteria

Concrete conditions that define done.

## Relevant Context

The current behavior, constraints, and business rules that matter.

## Relevant Files

- `exact/path/to/file`
- `exact/path/to/another-file`

For each file, explain why it matters.

## User Decisions

Restate the material product, scope, and UX choices the user made so a fresh agent does not need the chat history.

## Implementation Decisions

Document the architectural, validation, data-flow, and testing decisions that shape the implementation.

## Assumptions

Explicit assumptions that are not yet verified.

## Out of Scope

What this plan intentionally does not cover.

## Implementation Plan

- [ ] Step name
  Files: `exact/path/to/file.ts`, `exact/path/to/other-file.ts`
  Changes: describe the concrete outcome expected from this step
  Verification: command(s) to run and behavior to confirm

- [ ] Step name
  Files: `exact/path/to/file.ts`
  Changes: describe the concrete outcome expected from this step
  Verification: command(s) to run and behavior to confirm

## Risks

Key failure modes, regressions, migration concerns, or rollout issues.

## Docs To Update

List the matching docs that should change if implementation proceeds.

## Notes

Anything else an implementer should know.
```

## Task Granularity

- Each checkbox should be one meaningful, verifiable action.
- Prefer steps that are small enough to execute confidently, but large enough to avoid noise.
- Do not turn every shell command into its own checkbox unless it is an important checkpoint.
- Prefer vertical slices over horizontal "all backend, then all frontend, then all tests" plans.
- One plan should represent one coherent implementation slice. If the work requires multiple independently shippable slices, recommend multiple plans instead of stretching one plan across many phases.

## Testing Guidance

- Match the repo's existing testing patterns.
- Do not default to writing tests for everything.
- Include tests when behavior is meaningful, risky, or easy to regress.
- Prefer public-behavior tests for user-visible or boundary-level behavior.
- Add focused unit tests when logic is non-trivial and the narrower feedback is useful.
- Name the test files or areas that should be added or updated.
- Include the most relevant verification commands, usually a subset of:
  - lint
  - typecheck
  - unit tests
  - integration or acceptance tests
  - build

## Guardrails

- Do not edit product code while using this skill.
- Start from repo guidance when it exists, then confirm in code.
- Do not document aspirational architecture as if it already exists.
- Do not hide uncertainty; put it in `Assumptions`.
- Do not rely on the conversation for material implementation choices; restate them in `User Decisions` or `Implementation Decisions`.
- Do not leave file targets vague when the repo already makes them discoverable.
- Do not pad the plan with generic steps that say little more than "implement this."

## Avoid These Plan Failures

- `TODO`, `TBD`, or "implement later"
- "Add validation" without saying where and what validation
- "Write tests" without naming the behavior to protect
- Generic steps with no file targets
- Plans based on assumptions that were not checked in code
- Massive task groups that cannot be verified incrementally
- Plans that are so broad they should really be split into multiple implementation plans

## Final Handoff

After writing the plan:

- provide the saved path if a file was created
- say whether the plan is small enough for a fresh agent with no chat history
- summarize the main implementation steps
- call out any assumptions that should stay visible during implementation
- call out any user decisions that were captured in the plan
- ask whether to refine the plan or implement it
