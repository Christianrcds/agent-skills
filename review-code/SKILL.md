---
name: review-code
description: Review code changes for maintainability, efficiency, project guidelines, and reliability using four focused review passes. Use when the user wants a code review, PR review, change review, or a second opinion on code quality and risk.
---

# Review Code

Use this skill to review code through four explicit lenses:

1. maintainability
2. efficiency
3. project guidelines
4. reliability

Default to all four lenses unless the user narrows the scope.

Do not edit code unless the user explicitly asks for implementation after the review.

## Review Goal

Find the highest-value problems first:

- correctness bugs
- reliability risks
- meaningful maintainability problems
- project-guideline violations that matter
- inefficiencies likely to have real impact
- missing tests where behavior could regress

Do not fill the review with style-only nits or speculative complaints.

This review mindset should be informed by *Designing Data-Intensive Applications*: pay attention to data flow, boundaries, failure handling, correctness under real-world conditions, and whether the design will stay understandable as the system grows.

## Workflow

### 1. Establish Scope

Prefer reviewing the smallest clear unit that matches the request:

- current diff
- current branch changes
- a specific file or folder
- a commit
- a pull request

If the user gives a narrow scope, stay within it.

If no explicit scope is provided, infer it from the current changes and the user request.

### 2. Gather Local Guidance

Before reviewing, inspect the repo for the rules that matter:

- `AGENTS.md` or similar local instructions
- README or architecture docs
- linting, formatting, and test configuration
- contribution or style guidance
- patterns already used in the touched area

Treat repo conventions as first-class review criteria.

### 3. Run Four Focused Review Passes

If subagents are available and the scope is large enough to justify it, spawn four subagents in parallel.

Assign one lens to each subagent:

- Maintainability reviewer
- Efficiency reviewer
- Project-guidelines reviewer
- Reliability reviewer

Each subagent should inspect the same review scope, but only through its assigned lens.

If subagents are not available, perform the four passes sequentially yourself.

For detailed heuristics, read:

- [Review Lenses](references/review-lenses.md)

### 4. Validate And Merge Findings

After the four passes:

- deduplicate overlapping findings
- verify each finding against the code before reporting it
- discard weak or purely theoretical issues
- keep the strongest framing for each finding
- order findings by severity and user impact

Prefer fewer strong findings over many weak ones.

### 5. Report Findings

Present findings first.

For each finding, include:

- severity
- the affected file and line when possible
- the concrete risk
- why it matters

Then include:

- open questions or assumptions
- residual risks or testing gaps
- a short summary only after the findings

If there are no findings, say so explicitly and mention any residual risk or missing verification.

## Subagent Prompts

When spawning subagents, tell each one:

- its single assigned lens
- the exact review scope
- not to edit files
- to prioritize actionable findings over commentary
- to return only issues that materially matter
- to include file references when possible

The four subagents are not looking for the same thing:

- Maintainability: readability, cohesion, duplication, unclear abstractions, brittle structure, hard-to-change code
- Efficiency: wasteful queries, unnecessary work, slow paths, repeated computation, excessive rendering, needless allocations
- Project guidelines: violations of documented repo rules, architecture drift, testing-pattern mismatches, ignored local conventions
- Reliability: bugs, edge cases, incorrect assumptions, race conditions, state issues, error handling gaps, missing validation

## Guardrails

- Do not edit code during review unless the user explicitly asks for fixes.
- Do not invent project rules that the repo does not actually have.
- Do not report micro-optimizations without likely user or system impact.
- Do not confuse personal preference with a real maintainability issue.
- Do not over-index on style when reliability or correctness risks exist.
- Do not report a suspicion as a finding without checking the code path.
- Do not let the four-lens structure create four low-value sections if there are only one or two real issues.

## Final Check

- Are the findings actionable?
- Are they grounded in the actual code and repo guidance?
- Are the most important risks listed first?
- Did the review stay focused on meaningful issues?
- Would the user know what to fix next?
