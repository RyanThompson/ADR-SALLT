# 0016 Code Style

**Status:** Accepted · **Layer:** Laravel · **Principles:** P4 P8 · **Stack:** 0001

> Pint formats. Required state fails loud. Optional state is declared.

## Rules
1. Changed PHP passes Pint with the project config. The baseline is Pint's `laravel` preset.
2. Every `??`, `?:`, `?->`, `optional()`, `data_get`/`Arr::get` default, or empty-string/array/zero fallback maps to a value optional by design.
3. Required state is read directly or guarded by a named domain exception.
4. Optionality is explicit: nullable types, value objects or documented DTO fields.
5. New files declare `strict_types=1`. *(warning)*
6. Formatting-only changes are separate commits from behavior changes. *(warning)*

## Shape
```php
$name = $todo->name;                                         // required
$subtitle = $todo->description ?? __('domains/todos.no_description'); // optional by design
throw_unless($todo->organization_id, MissingTodoOrganization::class, $todo->id);
```

## Not
- `?? ''`, `?? []`, `?? 0` to quiet static analysis.
- `$group?->organization?->name ?? ''`.


Checklist: [`checklists/0016-code-style.yml`](../checklists/0016-code-style.yml)
