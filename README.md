# ADR-SALLT

The architecture canon for **SALLT**, the stack: **S**treams, **A**lpine.js, **L**aravel, **L**ivewire, **T**ailwind.

This is the north star. Code is aligned to it, reviews are measured against it, and the Streams SDK generates into it. If every rule here holds, the code is the code we meant to write.

## Reach order
**Single source of truth:** [`adr/0001`](adr/0001-salty-stack.md). Do not restate it elsewhere.

Streams → Alpine.js → Laravel → Livewire → Tailwind.

## Tiers
| Tier | Files | Answers |
|---|---|---|
| Business | [`000.0`](000.0-mission.md)–[`000.2`](000.2-guidance.md) | Why we exist, what shaped us, how we decide. **Templates** — each project answers them. Business-only. |
| Foundations | [`000.3`](000.3-foundations.md) | P1–P9 — testable principles that apply to anything **before** architecture. Business-only decimals with the templates above. |
| Salty stack | [`adr/0001`](adr/0001-salty-stack.md) | What the platform is, how to discover everything, and the lens for every architecture decision. |
| Architecture ADRs | [`adr/0002+`](adr/README.md) | How each concern is done through that lens. |
| Checklists | [`checklists/`](checklists/) | Is it done |
| Generation | [`generate/`](generate/README.md) | What the Streams SDK must produce into this canon |
| Gap map | [`RECONCILIATION.md`](RECONCILIATION.md) | ADR good vs SDK can-emit |
| Punch list | [`PUNCHLIST.md`](PUNCHLIST.md) | Dogfood-ordered generator backlog |
| Plan | [`PLAN.md`](PLAN.md) | Phased execution (approved) |

**Precedence:** Mission › Foundations (000.3) › Salty stack (0001) › architecture ADR › checklist › existing code. Existing code is evidence, not authority. An ADR that contradicts 000.3 or 0001 is wrong — fix the ADR.

## Streams SDK (how generation works)
Canon is not a pile of `make:*` Artisan commands. The Streams SDK (`streams/sdk`) centers on one app tree and a small set of commands:

| Piece | Role |
|---|---|
| **`streams.json`** (+ `$ref` files) | The app tree — streams and UI nodes addressed by dot path (`todos`, `todos.table`, `todos.pages.manage`) |
| **JSON Schema** | Self-describing: `$schema` on the tree and stream definitions; `streams:schema` emits schemas |
| **`streams:validate`** | Checks definitions against schema and the running app |
| **`streams:make`** | **The** generation command — edit the tree (prompts, flags, JSON recipe, menu, rename), then build PHP from it |
| **`streams:capture`** | Opposite of make — read existing panel builders into `streams.json` |
| **`KindRegistry`** | Component kinds (`stream`, `page`, `table`, `action`, `manage`, …) — data, not Artisan commands. Apps and packages register more kinds |

Legacy `make:stream` / `make:entry` / `make:addon` / `make:table` classes may exist in the package; they are **not** the primary surface (`php artisan list` shows `streams:make`, not a per-artifact `make:*` catalog). Details and gaps: [`generate/README.md`](generate/README.md).

## The Gate
A change ships when every line is true.
1. Its audience is named (000.2).
2. ADR 0001 and the ADRs governing the touched paths are named.
3. Every `error` rule in their checklists passes.
4. Every P1–P9 test in 000.3 passes for the change.
5. Static analysis, Pint and focused Pest tests are green.
6. Every deviation carries an exception comment.

## Exceptions
One format: an inline `ADR-{id} exception:` comment stating why, why the pattern does not fit, and what removes it. Principles take no exceptions — if one must bend, change the principle.

## Legacy
Grandfathered until touched. Touching a unit brings that unit to canon. Debt is never precedent.

## Adopting in a project
1. Copy `000.0`–`000.2` into the project's `docs/adr/` and answer every question.
2. Reference this repo for `000.3`, `adr/` and `checklists/` — do not copy and edit them.
3. Project-only decisions go in the project's `docs/adr/` numbered **0100+**. `0001`–`0099` are reserved for the canon.
4. To change the canon, change it here, then bump the reference in each project.

## Origin
Canon for the SALLT stack. Examples and the test bench use a **to-dos** app (`Todos`, `Projects`, `Labels`) — not product-specific domain code. `000.0`–`000.3` are business-only; architecture starts at ADR 0001 (salty stack), then Streams → Alpine → Laravel → Livewire → Tailwind. See [`adr/README.md`](adr/README.md).
