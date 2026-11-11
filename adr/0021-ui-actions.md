# 0021 UI Actions and Modals

**Status:** Accepted · **Layer:** Streams · Livewire · **Principles:** P2 P6 P7 · **Stack:** 0001

> A UI action owns the interaction. A domain action owns the change.

## Rules
1. Classes that open a modal end in `Modal`; others are named for their intent.
2. Mountable actions are registered by the owner (`register($livewire)`) before `resolve()`. Never `make()->livewire()`.
3. Mutations dispatch a domain action with a DTO. No inline persistence.
4. Never override `getDefaultName()`; pass an explicit name at the call site when one must differ.
5. Navigating table cells use `LinkColumn`; grouped actions use `ActionMenu`. *(warning)*

## Shape
```php
final class ArchiveTodoModal extends MountableAction
{
    protected function setUp(): void
    {
        $this->label(__('domains/todos.actions.archive.label'))
            ->modalHeading(__('domains/todos.actions.archive.heading'))
            ->authorize('delete', $this->livewire->group)
            ->action(fn () => dispatch_sync(new ArchiveTodo(ArchiveTodoData::fromEdge(
                group: $this->livewire->group,
                actor: auth()->user(),
                at: now()->toImmutable(),
            ))));
    }
}
```

## Not
- `AddTodoAction` for a modal.
- Full inline forms where a dedicated form exists.

Checklist: [`checklists/0021-ui-actions.yml`](../checklists/0021-ui-actions.yml)
