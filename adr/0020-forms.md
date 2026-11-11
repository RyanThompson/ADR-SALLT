# 0020 Forms

**Status:** Accepted · **Layer:** Streams · Livewire · **Principles:** P3 P6 P7 · **Stack:** 0001

> A form owns fields and validation, then hands validated state to a DTO.

## Rules
1. Forms are dedicated `Panel/Forms/*Form` classes.
2. Submit is wired through `handleForm('{form-name}')`.
3. `handle()` reads `getState()` — flat, validated, scoped to the form's own bucket.
4. The derived name and `statePath` are never overridden — not on the form, not on fields, not via `getDefaultName()`. Pass a different name at `register`/`resolve`/`for` if one is needed.
5. The submit path is: `getState()` → DTO factory → `dispatch_sync(Action)`. The form is an edge; it supplies actor, tenant and time to the DTO.
6. Field rules express validation; manual validation only for cross-field rules. *(warning)*

## Shape
```php
public function handle(array $payload = []): mixed
{
    $livewire = $this->getLivewire();

    dispatch_sync(new UpdateTodoTitle(UpdateTodoTitleData::fromForm(
        validated: $this->getState(),
        todoId: $livewire->todo->id,
        actor: auth()->user(),
        organizationId: UI::getTenant()->id,
        at: now()->toImmutable(),
    )));

    return null;
}
```

## Not
- `$this->statePath('data')` or `->statePath()` on fields.
- `data_get($validated, 'data.form.field')`.
- Page-level `save*()` for a dedicated form.


Checklist: [`checklists/0020-forms.yml`](../checklists/0020-forms.yml)
