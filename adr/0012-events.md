# 0012 Events and Listeners

**Status:** Accepted · **Layer:** Laravel · **Principles:** P1 P2 P3 · **Stack:** 0001

> Events are immutable facts. Listeners react through the owner's public actions.

## Rules
1. Names are Subject+PastVerb (`TodoCreated`). No imperatives, no `Was`/`Were`.
2. Events are `final readonly` with typed properties and live in the emitting domain's `Events/`.
3. Emitted only after the write commits.
4. Payloads carry the entity or ids, plus the DTO only if listeners need it. No raw request data or secrets.
5. A listener writes only through the owning domain's public action or contract.
6. A listener takes context from the event, never from `auth()`, `request()` or `now()`.
7. Listeners are registered explicitly and idempotent when queued.

## Shape
```php
final readonly class TodoTitleUpdated
{
    public function __construct(public Todo $todo, public UpdateTodoTitleData $data) {}
}

final class NotifyAssigneesOfRename
{
    public function handle(TodoTitleUpdated $event): void
    {
        dispatch(new SendTodoNotice(SendTodoNoticeData::fromEvent($event)));
    }
}
```

## Not
- `TodoWasCreated`, `TodoChanged`.
- Listeners writing another domain's streams.
- Business order that can only be understood by reading a chain of listeners.


Checklist: [`checklists/0012-events.yml`](../checklists/0012-events.yml)
