# Agents

Before writing code in a SALLT project:

1. Read the project's `000.0`–`000.2` (business answers).
2. Read canon `000.3` here — foundations P1–P9 only.
3. Read ADR [`0001`](adr/0001-salty-stack.md) — the salty stack lens (reach order, platform, discovery). **Do not** take reach order from anywhere else.
4. Name the audience (000.2) and the architecture ADRs (`0002+`) that govern the paths you will touch.
5. Generate with the Streams SDK (`streams:make`, KindRegistry) where a kind/recipe exists ([`generate/`](generate/README.md)); otherwise copy the stub.
6. Before returning, pass every `error` rule in the governing `checklists/` (always include 0001 for stack concerns). Mark any deviation `ADR-{id} exception:` with why and what removes it.

Gap map: [`RECONCILIATION.md`](RECONCILIATION.md). Dogfood order: [`PUNCHLIST.md`](PUNCHLIST.md). Plan: [`PLAN.md`](PLAN.md).

Existing code is evidence, not authority. When it disagrees with the canon, the canon wins — fix the unit you touch.
