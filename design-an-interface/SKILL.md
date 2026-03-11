---
name: design-an-interface
description: Generate multiple radically different interface designs for a module and compare tradeoffs before implementation. Use when designing APIs, module boundaries, or public contracts.
---

# Design an Interface

Use "design it twice" (or more): first idea is rarely best.

## Workflow

1. Gather requirements:
   - Problem to solve
   - Primary callers
   - Critical operations
   - Constraints (performance, compatibility, existing patterns)
2. Produce at least 3 materially different interface options.
3. Show usage examples for each option.
4. Compare tradeoffs:
   - Simplicity
   - Misuse resistance
   - Extensibility
   - Internal complexity hidden behind interface
5. Recommend one option and explain why.

## Option Format

For each design, include:

1. Interface signature
2. Example usage
3. What stays hidden internally
4. Tradeoffs

## Guardrails

- Do not produce superficial variations.
- Do not jump to implementation during design phase.
- Optimize for clear boundaries and maintainability.
