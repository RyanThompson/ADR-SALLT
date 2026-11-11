# Plan — Salty stack ready to use

**Status:** Approved and executing.  
**Intent:** Pin version-controlled dependencies, then work on them **from the 🧂 SALLT create-project** ([RyanThompson/SALLT](https://github.com/RyanThompson/SALLT) → local `Sites/SALLT`).

| Phase | Outcome |
|---|---|
| 1 | [`RECONCILIATION.md`](RECONCILIATION.md) + [`PUNCHLIST.md`](PUNCHLIST.md) landed |
| 2 | [`adr/0001`](adr/0001-salty-stack.md) sole stack home; [`000.3`](000.3-foundations.md) = P1–P9 only |
| 3 | **SALLT** starter: `composer create-project ryanthompson/sallt` → Laravel shell + SDK → `sallt:onboard` TUI → generators |

After scaffold: implement onboard TUI + generators in punch-list order against real failures in `SALLT`.

---

## Phase 1 — Close the loop on in-flight work

Land the **SDK reconciliation report** as the gap map between ADR “good” and what `streams-sdk` can emit.

1. Missing-generators list  
2. Scalability / maintainability audit  
3. Registry design (open)

**Done when:** report filed in-repo and treated as the only ordered backlog before implementation.

---

## Phase 2 — ADR 0001 as single source of truth

Make **`adr/0001-salty-stack.md`** the only home for reach order, platform definition, and discovery.  
**`000.3`** keeps P1–P9 only; Part 2 replaced with a pointer. Architecture ADRs cite **Stack: 0001**.

---

## Phase 3 — To-dos app as dogfood

App: **`/Users/ryanthompson/Sites/streams-todo`**.

| Piece | Action |
|---|---|
| ADR-SALLT | Symlinked at `packages/adr-sallt` → `_packages/ADR-SALLT` |
| streams-sdk | Composer path dep, pinned to local checkout commit |
| `000.0`–`000.2` | Filled under `docs/adr/` with to-dos mission/context/guidance |
| AI start | `AGENTS.md` points at canon + 0001 lens |

---

## After the scaffold runs

Follow [`PUNCHLIST.md`](PUNCHLIST.md) — P0 domain recipe + validated `streams.json` first.
