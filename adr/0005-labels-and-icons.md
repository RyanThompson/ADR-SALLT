# 0005 Labels and Icons

**Status:** Accepted · **Layer:** Streams · **Principles:** P9 · **Stack:** 0001

> An action shows text or an icon. Both only when the icon says something the text does not.

## Rules
1. A visible action shows a translated label or an icon (`->label(false)`), not both.
2. No icon that restates its label (View + eye, Edit + pencil, Done + check).
3. Named product verbs are text-only.
4. Icon-only actions use unambiguous glyphs and keep an accessible name.
5. Label + icon is allowed only for added meaning (e.g. opens in a new context).
6. When unsure, omit the icon. *(warning)*

## Shape
```php
Action::make('view')->label(__('domains/todos.actions.view'));                 // text
Action::make('edit')->label(false)->icon('heroicon-o-pencil');                       // icon
Action::make('preview')->label(__('…'))->icon('heroicon-o-arrow-top-right-on-square'); // icon adds meaning
```

## Not
- Icons added "for polish".

Checklist: [`checklists/0005-labels-and-icons.yml`](../checklists/0005-labels-and-icons.yml)
