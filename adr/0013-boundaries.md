# 0013 Boundaries

**Status:** Accepted · **Layer:** Laravel · **Principles:** P1 P5 · **Stack:** 0001

> Each canonical entity has one owner. Cross the boundary by asking or listening — never by writing.

## Rules
1. Name the owner before writing mutation code.
2. Never write another domain's streams, entries, models or repositories.
3. Synchronous cross-domain work calls the owner's public action or `Contracts/` interface.
4. Asynchronous side effects use events and listeners (0012).
5. Orchestrating another domain's capability does not transfer ownership.
6. Projections and read models are rebuildable and never update canonical state (0017).

## Shape
```php
// Todos screen attaching labels — Labels owns labels.
dispatch_sync(new AttachLabels(AttachLabelsData::forTodo($todo, $tagIds, $actor, $at)));   // yes
entries('labelables')->create([...]);                                                   // no
```

## Not
- Importing another domain's model and calling `save()`, `update()`, `delete()` or `create()`.
- Listeners as a loophole for cross-domain writes.


Checklist: [`checklists/0013-boundaries.yml`](../checklists/0013-boundaries.yml)
