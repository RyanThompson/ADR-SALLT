# 0019 Pages

**Status:** Accepted · **Layer:** Streams · Livewire · **Principles:** P3 P7 P9 · **Stack:** 0001

> A page is a composition root. It loads context, then lists what it shows.

## Rules
1. Pages extend the Streams UI panel page and compose through `getComponents()`.
2. Pages hold no mutation, validation or query logic beyond loading their own entry.
3. All Livewire state lives in the shared `$data` bucket. No parallel public state arrays.
4. Tenant comes from `UI::getTenant()`. Never re-resolve it from `auth()` or by re-fetching.
5. Pages are routed only through their resource's `getPages()`.
6. Resource tabs set `$data['active_navigation']` in `boot()`; items use `isActiveWhen()`. *(warning)*
7. The page H1 carries no description unless it is entity content or the message itself. *(warning)*

## Shape
```php
final class TodoSettings extends PanelPage
{
    protected static string $resource = TodosPanelResource::class;

    public function boot(): void
    {
        $this->todo = entries('todos')->findOrFail($this->entry);
        $this->data['active_navigation'] = 'settings';
    }

    public function getComponents(): array
    {
        return [
            self::$resource::breadcrumbs(),
            Heading::make($this->todo->name)->priority('h1'),
            self::$resource::navigation($this->todo->id, 'settings'),
            ManageTodoDetailsCard::for($this),
            ManageTodoChecklistCard::for($this),
        ];
    }
}
```

## Not
- `save*()` methods on a page.
- Inline table or form schemas.
- `abort(404)` after `find()` where `findOrFail()` exists.


Checklist: [`checklists/0019-pages.yml`](../checklists/0019-pages.yml)
