# Deep Modules

From "A Philosophy of Software Design":

**Deep module** = small interface + lots of implementation

```text
┌─────────────────────┐
│   Small Interface   │  ← Few methods, simple params
├─────────────────────┤
│                     │
│                     │
│  Deep Implementation│  ← Complex logic hidden
│                     │
│                     │
└─────────────────────┘
```

**Shallow module** = large interface + little implementation (avoid)

```text
┌─────────────────────────────────┐
│       Large Interface           │  ← Many methods, complex params
├─────────────────────────────────┤
│  Thin Implementation            │  ← Just passes through
└─────────────────────────────────┘
```

## Questions To Ask While Designing

- Can I reduce the number of methods?
- Can I simplify the parameters?
- Can I hide more complexity inside?

## General Guidance

- Prefer modules that expose a small public surface with meaningful behavior inside.
- Avoid thin orchestration layers that only pass data from one helper to the next without absorbing any complexity.
- Do not split files just to create more layers. A single deeper module is often better than multiple shallow wrappers.
- When a caller has to know too many steps in the right order, the module is probably too shallow.
- Keep presentation and transport layers simple, but do not leak business rules into them without a good reason.
