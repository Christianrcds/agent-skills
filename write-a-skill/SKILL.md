---
name: write-a-skill
description: Create new agent skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new skill.
---

# Writing Skills

## Process

1. Gather requirements:
   - What task/domain the skill covers
   - Main use cases and triggers
   - Whether scripts are needed for deterministic tasks
2. Draft the skill:
   - `SKILL.md` with concise instructions
   - Optional references for rarely needed details
   - Optional scripts for repeatable operations
3. Review and refine:
   - Check missing scenarios
   - Remove unnecessary verbosity
   - Tighten trigger wording

## Skill Structure

```text
skill-name/
└── SKILL.md
```

Optional when needed:

```text
skill-name/
├── SKILL.md
├── references/
└── scripts/
```

## SKILL.md Template

```md
---
name: skill-name
description: What it does. Use when [clear triggers].
---

# Skill Name

## Quick start

[Minimal, practical workflow]

## Guardrails

[Critical do/don't rules]
```

## Description Rules

- First sentence: capability.
- Second sentence: clear trigger conditions (`Use when ...`).
- Be specific enough to disambiguate from other skills.

## Quality Bar

- Keep it concise; avoid generic filler.
- Prefer checklists and concrete steps.
- Split into references only when content grows large.
