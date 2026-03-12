---
name: create-doc
description: Document a completed feature in the existing docs using final behavior, key files, and current flow only. Use when a feature is done and the user wants durable engineering docs or future AI context.
---

# Create Doc

## Quick start

1. Find the canonical existing docs area for the feature.
2. Prefer adding a focused doc in that area plus an index entry instead of burying the behavior in a large unrelated file.
3. Write only the final behavior.
4. Include the files an engineer or agent should read first.
5. Keep the doc useful for both engineers and future AI context gathering.

## Required output

Every feature doc should cover:

- purpose
- what the feature does
- how it works
- API or UI behavior that matters operationally
- persistence/runtime behavior when relevant
- key files

## Writing rules

- Do not write changelog language.
- Do not describe previous behavior.
- Do not explain implementation history or why a choice was made unless explicitly requested.
- Prefer direct statements such as:
  - `It works this way`
  - `The UI does...`
  - `The backend loads...`
  - `The route returns...`
- Avoid phrases like:
  - `now it works like this`
  - `previously`
  - `we changed`
  - `this was added because`

## Preferred doc shape

Use concise sections like:

- `Purpose`
- `Product behavior`
- `Frontend behavior`
- `Backend API surface`
- `Persistence model`
- `Runtime behavior`
- `Key files`

## Placement rules

- Reuse the existing domain docs folder when one exists.
- Update that folder’s index or README so future agents find the new doc from the canonical entrypoint.
- If an existing document is the canonical architecture reference, link to it instead of duplicating broad context.

## Guardrails

- Optimize for future lookup, not narrative prose.
- Include exact file paths when they help navigation.
- Keep the doc scoped to one feature or one cohesive behavior.
- If the feature crosses frontend and backend, document both sides in one place when that improves discoverability.
