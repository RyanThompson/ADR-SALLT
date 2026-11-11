# 0004 Tables

**Status:** Accepted · **Layer:** Streams · **Principles:** P3 P7 P9 · **Stack:** 0001

> A table owns its query, columns, filters, actions and empty state.

## Rules
1. Tables are dedicated `Panel/Tables/*Table` classes.
2. The table configures its own query, columns, filters, row/bulk/header actions and empty state.
3. Queries scope by `UI::getTenant()` with explicit `where($field, '=', $value)`.
4. Section titles go on the table (`heading()`, `description()`, `headerActions()`), never on a wrapping `Card`.
5. One table per use case. Extract shared columns only when reuse is real. *(warning)*

## Shape
```php
final class TodoActivityTable extends Table
{
    protected function setUp(): void
    {
        $this->heading(__('domains/todos.logs.heading'))
            ->query(entries('todo_activity')->where('organization_id', '=', UI::getTenant()->id))
            ->columns([
                TextColumn::make('created_at')->label(__('domains/todos.logs.columns.when'))->sortable(),
            ])
            ->emptyStateHeading(__('domains/todos.logs.empty.heading'));
    }
}
```

## Not
- Inline column schemas on a page.
- Table styling through an app view override.


Checklist: [`checklists/0004-tables.yml`](../checklists/0004-tables.yml)
