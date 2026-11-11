# 0008 Alpine.js

**Status:** Proposed · **Layer:** Alpine · **Principles:** P1 P6 P8 · **Stack:** 0001

> Native HTML first, Alpine second, server last — for behavior only the browser needs.

## Rules
1. Use native HTML before Alpine: `<details>`, `<dialog>`, `popover`, form constraint attributes, anchors.
2. Alpine owns ephemeral UI state only: open/closed, local tabs, dirty flags, transitions, focus, keyboard, clipboard.
3. Alpine never fetches domain data, holds server truth, enforces business rules or authorizes.
4. State lives in Alpine or in Livewire `$data`, never both. No `@entangle` / `$wire.entangle`; if both sides need it, the server owns it.
5. Inline `x-data` holds one line of state. Anything larger or reused is `Alpine.data('{domain}{Name}', …)`, registered in the one JS entry file.
6. Server calls go through Livewire (`wire:*`, `$wire`). No `fetch` to app routes from Alpine.
7. No other client framework — cite and follow 0001; wrap third-party JS once as an Alpine component fed by server-rendered data.
8. On Streams components, Alpine attributes are added through `mergeHtmlAttributes()`, never by forking markup.

## Shape
```php
// Ephemeral state on a Streams form — inline, one line.
$this->mergeHtmlAttributes(['x-data' => '{ dirty: false }', 'x-on:input' => 'dirty = true']);
```
```js
// resources/js/app.js — the one registration point.
Alpine.data('todosBoardFilters', (filters = {}) => ({
    open: false,
    filters,
    toggle() { this.open = ! this.open },
}));
```

## Not
- A Livewire round-trip to open a dropdown.
- `x-data` objects with methods inline in PHP strings.
- Alpine calling `fetch('/api/...')` for domain data.


Checklist: [`checklists/0008-alpine.yml`](../checklists/0008-alpine.yml)
