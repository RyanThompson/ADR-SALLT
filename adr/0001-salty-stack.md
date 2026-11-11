# 0001 Salty Stack (SALLT)

**Status:** Accepted · **Layer:** Streams · Alpine · Laravel · Livewire · Tailwind · **Principles:** P1–P9

> SALLT is the platform and the lens: Streams, Alpine.js, Laravel, Livewire, Tailwind — in that reach order. Every architecture decision is made through this stack.

This ADR is the **single source of truth** for reach order, platform ownership, and discovery. Other ADRs cite it; they do not restate it. Foundations ([`000.3`](../000.3-foundations.md)) hold P1–P9 only.

## Rules
1. Solve each concern at the first layer that can own it. Take the next layer only when the one above cannot do the job.

| # | Layer | Owns | Never owns |
|---|---|---|---|
| 1 | **Streams** (core, ui, api, sdk) | Data: stream definitions, entries, fields, validation. UI: panels, resources, pages, forms, tables, actions, components, navigation, tenancy. REST: interfaces, resources, endpoints. Generation. | Business rules (→ domain actions) |
| 2 | **Alpine.js** | Behavior only the browser needs: disclosure, transitions, focus, keyboard, clipboard, unsaved-changes flags — in native HTML | Server truth, fetching domain data, business rules, authorization |
| 3 | **Laravel** | Runtime: container, routing, queues, events, policies, auth, mail, scheduling. The domain core: actions, DTOs, events, listeners, policies | UI composition (→ Streams UI) |
| 4 | **Livewire** | Interactive state the server must hold, hosted by Streams UI pages, in one `$data` bucket | Mutation (→ actions); state the browser alone needs (→ Alpine) |
| 5 | **Tailwind** | Appearance the Streams UI theme does not already provide | Structure; package markup |

2. Streams is the platform. If Streams does it, use it. If Streams should do it, add it upstream. The app is a consumer of Streams.
3. Alpine owns behavior only the browser needs, in native HTML. It never round-trips for that behavior.
4. Laravel is the runtime and the domain core. Domain logic knows nothing about the UI that calls it.
5. Livewire holds interactive state the server must know, in one `$data` bucket.
6. Tailwind styles what the theme does not already give. Theme and variants before utilities.
7. Discover stack law **here** first, then the ADRs that govern the touched paths (`adr/0002+`), then their checklists.
8. **Data is a stream.** A new persisted concept is a stream definition, read and written through `entries('{stream}')`. An Eloquent model needs an `ADR-0001 exception:`.
9. **Behavior is a domain action.** No business rule lives in UI, API, Livewire or Alpine. (0010)
10. **UI is composed, not authored.** Reach in order: existing builder → builder option → app component composing builders (0003) → upstream change to Streams (0002). Custom Blade never duplicates a builder.
11. **One owner per piece of state.** Browser alone → Alpine. Server must know → Livewire `$data`. Never both. (0008)
12. **No other client framework.** No jQuery, Vue or React. Third-party JS is wrapped once as an Alpine component, fed by the server. (0008)
13. **Theme before utilities.** Tokens and component variants before classes; classes before custom CSS. (0022)
14. **One tenant resolver per surface.** Panel: `UI::getTenant()`. API: `API::getTenant()`. Async: the event or job payload. (P3)
15. Generic capability or fix → upstream Streams, then bump. App-specific capability → app domains. SDK gaps → [`generate/`](../generate/README.md). Integration with another product → its own ADR.

## Why this order
- **Streams first.** The platform is the leverage. Every gap filled in the app is a gap every other app still has. Streams does not require Laravel for a headless JS client; Alpine is the natural next layer.
- **Alpine before the server.** A toggle that round-trips is latency, load and state to keep in sync. Native HTML first, Alpine second.
- **Laravel is the core, not the surface.** Domain logic is plain PHP on Laravel primitives.
- **Livewire only when the server must know.** Bridge between UI and domain, not a second place for logic.
- **Tailwind last.** If the theme or a component variant can express it, a utility class is duplication.

## Shape
```text
000.0–000.2   business templates (project answers)
000.3         foundations — P1–P9 only (before architecture)
0001          this ADR — salty stack lens (architecture starts here)
0002+         architecture ADRs through the lens
checklists/   is it done
generate/     what the Streams SDK must produce
```

```bash
# Discovery for agents
# 1. Project 000.0–000.2 → audience and constraints
# 2. Canon 000.3 → P1–P9
# 3. This file → reach order and platform
# 4. Governing adr/0002+ → how
# 5. checklists/ → done
# 6. generate/ + streams:make → emit
```

## Not
- Reaching a lower layer when a higher one owns the concern.
- Treating Laravel or Livewire as the product surface over Streams.
- Restating this ADR’s reach order inside another ADR instead of citing `0001`.
- Inventing a parallel stack (another client framework, forked package UI, Eloquent-first persistence without an exception).

## Test
- [ ] Each touched concern sits in the first layer that can own it.
- [ ] New persisted data is a stream definition.
- [ ] No business rule above the action layer.
- [ ] No state held by both Alpine and Livewire.
- [ ] UI is built from Streams builders; no forked package views.
- [ ] Generic gaps went upstream to Streams.
- [ ] Governing ADRs were found via this lens, not by scanning the whole tree first.

Checklist: [`checklists/0001-salty-stack.yml`](../checklists/0001-salty-stack.yml)
