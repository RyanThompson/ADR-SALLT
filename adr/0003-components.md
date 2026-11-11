# 0003 Components

**Status:** Accepted · **Layer:** Streams · **Principles:** P2 P7 P9 · **Stack:** 0001

> A component owns its internals and is ready to render when you get it.

## Rules
1. Names end in their container: `Card`, `Section`, `Fieldset`, `Container`, `Widget`, `StatsGrid`.
2. Context-aware components are built with `for($livewire, ...)`. `make()` is for context-free construction only.
3. A component registers its own forms, tables and actions in `setUp()`; callers never register a child's internals.
4. Register before resolve (applies to every registry-backed builder).
5. Never rebuild an existing component, form or table inline.
6. A wrapper must add behavior. No `Card` solely to title a `Table`.

## Shape
```php
final class ManageTodoDetailsCard extends Card
{
    protected function setUp(): void
    {
        TodoTitleForm::register($this->getLivewire());

        $this->heading(__('domains/todos.settings.profile.heading'))
            ->components(fn () => [
                TodoTitleForm::resolve()->fill(['name' => $this->getLivewire()->group->name]),
            ]);
    }
}
```

## Not
- `Card::make()->livewire($this)`.
- `TodoComponent` when the container is known.
- A parent page registering a card's forms.


Checklist: [`checklists/0003-components.yml`](../checklists/0003-components.yml)
