# 0017 Derived Data

**Status:** Accepted · **Layer:** Laravel · Streams · **Principles:** P1 P5 P6 P9 · **Stack:** 0001

> Facts live in their owners. Metrics are deduced from facts, materialized for serving, and rebuildable.

## Rules
1. Three layers: facts (owning domains) → calculators (deduce for a closed period) → materialized values (serve).
2. No new fact stream when an existing timestamp or row already answers the question.
3. Dashboards serve catalogued metrics from materialized values, never live scans.
4. Calculators write closed periods only, day grain first.
5. One registered calculator per metric key.
6. If an as-of value cannot be rebuilt, the owner records history or the limit is documented.
7. Materialized values can be deleted and recomputed and never write back.

## Shape
```text
Need                                   → Do
Count over durable timestamps          → calculator over facts, daily
Mutable state as of a past day         → owner history, or daily snapshot + documented limit
Sparkline / % vs prior period          → serve from materialized values
```

## Not
- "Todos completed today" counters duplicating `completed_at`.
- Treating today's partial day as closed.


Checklist: [`checklists/0017-derived-data.yml`](../checklists/0017-derived-data.yml)
