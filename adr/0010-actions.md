# 0010 Actions

**Status:** Accepted · **Layer:** Laravel · **Principles:** P2 P3 P7 · **Stack:** 0001

> Every change to canonical state is one Action: one DTO in, one fact out.

## Rules
1. An Action is a command. Its constructor takes exactly one `*Data` DTO. No arrays, scalars or `Request`.
2. It executes in `__invoke()` and is called through `dispatch_sync()` or `dispatch()` — never `->__invoke()` or `(new X)()`.
3. It enforces authorization through the actor in its DTO (0014).
4. It emits one or more past-tense events after the write commits (0012).
5. It never reads `auth()`, `request()`, `session()`, `now()` or `Carbon::now()` (P3).
6. Its name is a specific imperative Verb+Noun. Split when fields save independently or differ in authorization, events or retry.
7. A read is not an Action. It is a named query in `Queries/` (`Find*`, `Resolve*`, `Estimate*`) and emits nothing.
8. Multi-write actions use a transaction. Retryable actions document idempotency. *(warning)*

## Shape
```php
final class UpdateTodoTitle
{
    public function __construct(private UpdateTodoTitleData $data) {}

    public function __invoke(): Todo
    {
        $todo = entries('todos')->find($this->data->todoId);
        Gate::forUser($this->data->actor)->authorize('update', $todo);

        $todo->fill(['title' => $this->data->title])->save();

        event(new TodoTitleUpdated($todo, $this->data));

        return $todo;
    }
}

dispatch_sync(new UpdateTodoTitle($data));
```

## Not
- `Process`, `Handle`, `Save`, `Manage` names.
- Persistence in pages, forms, endpoints, listeners or builders.
- Events emitted before the write succeeds.


Checklist: [`checklists/0010-actions.yml`](../checklists/0010-actions.yml)
