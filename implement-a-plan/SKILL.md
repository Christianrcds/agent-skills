---
name: implement-a-plan
description: Execute an approved implementation plan in small, verifiable slices and keep the plan current as work progresses. Use when the user explicitly asks to implement a written plan or approved execution roadmap.
---

# Implement a Plan

Announce at start: "I'm using the implement-a-plan skill to execute the approved plan."

## Goal

Implement an approved plan in small, verifiable slices.

Use the plan as the execution contract, but confirm it still matches the current codebase before changing code.

Assume the implementation may be picked up by a fresh agent with no prior chat history. If the plan is not specific enough to execute safely on that basis, update the plan before coding.

## Supported Inputs

This skill can execute from:

- a saved plan file in the repo
- a plan pasted into the conversation
- a previously approved implementation roadmap from the current thread

Prefer a saved plan file when the repo already has a planning workflow or when the work is large enough to benefit from a durable handoff.

## Workflow

1. Load the plan first.
   - Read the target plan completely before editing code.
   - Identify the goal, success criteria, relevant files, user decisions, implementation decisions, verification commands, risks, and docs follow-up.
   - Confirm the user explicitly asked to implement the plan.

2. Validate plan completeness before coding.
   - Confirm the plan is specific enough for a fresh agent with no chat history.
   - If key file targets, execution steps, or verification expectations are missing, update the plan before coding.
   - If the plan is really multiple independently shippable slices bundled together, pause and recommend splitting it before implementation continues.

3. Reconfirm the current codebase.
   - Re-read the relevant docs, local instructions, and active implementation before starting the first slice.
   - If the code has materially drifted from the plan, pause and resolve that mismatch before coding.
   - Minor execution details may change, but the implemented outcome should stay aligned with the plan.

4. Execute one small slice at a time.
   - Prefer one checkbox or one tightly-related group of checkboxes per cycle.
   - Keep each slice independently verifiable.
   - If the plan exists as a file, update it as work progresses:
     - set status to `in-progress` when execution begins
     - mark completed checkboxes
     - update `updated: YYYY-MM-DD`
     - record important deviations in a short note if execution differs from the original plan

5. Use TDD when the slice justifies it.
   - Follow the `tdd` skill for non-trivial behavior, branching logic, validation, orchestration, or transformations.
   - Prefer one failing behavior slice, then the minimum code to pass, then refactor while green.
   - Do not cargo-cult TDD into low-value tests for trivial passthrough code, type plumbing, or low-risk presentation-only changes.

6. Verify before moving on.
   - Run the most relevant tests and checks for the slice you just completed.
   - Use the plan's verification steps when they are still valid.
   - Before finishing the overall task, run the relevant repo-level checks from the plan.

7. Finish the plan cleanly.
   - Mark completed steps in the plan if it is stored as a file.
   - Set the plan status to `done` when the scoped work is complete.
   - If only part of the plan was implemented, leave the status honest and explain what remains.

## Execution Rules

- Do not start implementation unless the user explicitly asked for it.
- Read the plan before editing code.
- Treat the plan as the source of execution scope, unless the user changes the scope.
- If the plan and the code disagree in a meaningful way, resolve the mismatch before proceeding.
- Keep the plan updated as a living execution record when the plan is stored in-repo.
- Keep docs in sync when the implementation changes documented behavior.

## TDD Guidance

- Use the repo's existing testing patterns and the `tdd` skill when useful.
- Use `tdd` as a decision gate, not a blanket rule.
- Let `tdd` drive the deep-module and refactor decisions during each green cycle when the slice has meaningful behavior risk.
- Prefer public-behavior tests for boundary and user-visible behavior.
- Add focused unit tests when logic is non-trivial and the narrower feedback is useful.
- Mock only system boundaries, not internal business logic in the same behavior slice.
- Prefer small, behavior-focused cycles over broad speculative implementation.

## Plan Update Expectations

When executing from a saved plan file, keep it current:

- frontmatter status reflects reality
- completed checkboxes are checked
- notable deviations are recorded briefly
- docs follow-up is actually followed when behavior changes

Do not silently leave the plan stale.

If the plan exists only in chat, keep the same discipline in your progress updates and final handoff.

## Guardrails

- Do not blindly follow a stale plan.
- Do not start coding from a plan that would confuse a fresh agent with no chat history.
- Do not batch too many unchecked steps into one large change.
- Do not skip verification because the plan looked clear.
- Do not create shallow abstraction layers just to match a planned file split.
- Do not refactor for aesthetics alone; refactor to improve clarity, cohesion, and maintainability.
- Do not silently broaden scope because implementation feels straightforward.

## Final Handoff

When implementation is complete:

- summarize what was implemented
- list any deviations from the original plan
- report the verification you ran
- call out any remaining unchecked items or follow-up work
