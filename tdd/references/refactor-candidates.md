# Refactor Candidates

After each green slice, look for:

- **Duplication** → Extract a function or module
- **Long methods** → Break into focused helpers while keeping tests on the public interface
- **Shallow modules** → Combine them or move more behavior behind a smaller interface
- **Feature envy** → Move logic closer to where the data lives
- **Primitive obsession** → Introduce a value object or clearer domain type when it improves meaning
- **Existing code** the new work reveals as problematic

## Guardrails

- Refactor only while tests are green.
- Prefer improving cohesion over introducing more layers.
- Do not move tests toward private helpers; keep tests on public behavior.
