# ADRs

Each ADR: one law, numbered rules, one shape, what not to do. Every rule maps 1:1 to its checklist.

**Before architecture:** [`000.0`](../000.0-mission.md)–[`000.2`](../000.2-guidance.md) (business templates), [`000.3`](../000.3-foundations.md) (P1–P9).

**Architecture starts at 0001** — the salty stack lens. Then Streams → Alpine → Laravel → Livewire → Tailwind. Examples use a **to-dos** bench app (`Todos`, `Projects`, `Labels`).

| # | ADR | Layer | Status | Law |
|---|---|---|---|---|
| 0001 | [Salty Stack (SALLT)](0001-salty-stack.md) | Streams · Alpine · Laravel · Livewire · Tailwind | Accepted | SALLT is the platform and the lens: Streams, Alpine.js, Laravel, Livewire, Tailwind — in that reach order. |
| 0002 | [Package Fidelity](0002-package-fidelity.md) | Streams | Accepted | Streams is fixed in Streams. The app never shadows a package. |
| 0003 | [Components](0003-components.md) | Streams | Accepted | A component owns its internals and is ready to render when you get it. |
| 0004 | [Tables](0004-tables.md) | Streams | Accepted | A table owns its query, columns, filters, actions and empty state. |
| 0005 | [Labels and Icons](0005-labels-and-icons.md) | Streams | Accepted | An action shows text or an icon. Both only when the icon says something the text does not. |
| 0006 | [REST API](0006-rest-api.md) | Streams · Laravel | Accepted | The API is a second edge onto the same domains: same scope, same actions, explicit routes. |
| 0007 | [Addons](0007-addons.md) | Laravel · Streams | Accepted | An addon is a Composer package that registers capabilities. It never owns core. |
| 0008 | [Alpine.js](0008-alpine.md) | Alpine | Proposed | Native HTML first, Alpine second, server last — for behavior only the browser needs. |
| 0009 | [Domains](0009-domains.md) | Laravel | Accepted | Code is organized by business capability. Each domain owns its data, behavior and surfaces. |
| 0010 | [Actions](0010-actions.md) | Laravel | Accepted | Every change to canonical state is one Action: one DTO in, one fact out. |
| 0011 | [Data Transfer Objects](0011-data-transfer-objects.md) | Laravel | Accepted | Input is a typed, immutable DTO built at the edge. Nothing is guessed. |
| 0012 | [Events and Listeners](0012-events.md) | Laravel | Accepted | Events are immutable facts. Listeners react through the owner's public actions. |
| 0013 | [Boundaries](0013-boundaries.md) | Laravel | Accepted | Each canonical entity has one owner. Cross the boundary by asking or listening — never by writing. |
| 0014 | [Authorization](0014-authorization.md) | Laravel | Proposed | Who may act is a domain rule, decided by the owner's policy and enforced by the action. |
| 0015 | [Copy and Translation](0015-copy.md) | Laravel | Accepted | Every user-facing word is a translation key in its domain's vocabulary. |
| 0016 | [Code Style](0016-code-style.md) | Laravel | Accepted | Pint formats. Required state fails loud. Optional state is declared. |
| 0017 | [Derived Data](0017-derived-data.md) | Laravel · Streams | Accepted | Facts live in their owners. Metrics are deduced from facts, materialized for serving, and rebuildable. |
| 0018 | [Webhooks](0018-webhooks.md) | Laravel | Accepted | Outbound webhooks follow Standard Webhooks v1 exactly. |
| 0019 | [Pages](0019-pages.md) | Streams · Livewire | Accepted | A page is a composition root. It loads context, then lists what it shows. |
| 0020 | [Forms](0020-forms.md) | Streams · Livewire | Accepted | A form owns fields and validation, then hands validated state to a DTO. |
| 0021 | [UI Actions and Modals](0021-ui-actions.md) | Streams · Livewire | Accepted | A UI action owns the interaction. A domain action owns the change. |
| 0022 | [Styling](0022-styling.md) | Tailwind | Proposed | Theme first, utilities second, custom CSS last — and never on package markup. |

## Open
| Gap | Next |
|---|---|
| Testing standard — what every action, form and endpoint must prove | Draft as 0023 |
| Domain shape: `Repositories/` in 0009 | Shape updated; SDK domain recipe still ❌ — see PUNCHLIST |
| Headless / JS Streams client | Draft Streams-surface ADR |

## Bench
Shapes and stubs use a to-dos app as the dummy project. Prefer `Todos` / `Todo` / `todoId` over product-specific domains.
