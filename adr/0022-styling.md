# 0022 Styling

**Status:** Proposed · **Layer:** Tailwind · **Principles:** P8 P9 · **Stack:** 0001

> Theme first, utilities second, custom CSS last — and never on package markup.

## Rules
1. Theme before utilities (0001): use Streams UI component variants and theme tokens (`->color('primary')`) before utility classes.
2. Utilities style app-owned markup only; package markup is changed upstream (0002).
3. No arbitrary values (`text-[13px]`, `#187bd8`) where a scale token exists. Missing tokens are added to the theme.
4. Custom CSS states its reason in a comment. No `@apply` to recreate a component — build the component.
5. Responsive and dark variants use Tailwind variants, not hand-written media queries. *(warning)*

## Shape
```php
Action::make('save')->color('primary');                              // variant
Card::make()->mergeHtmlAttributes(['class' => 'lg:col-span-2']);     // layout utility on app composition
```

## Not
- Hex colors in PHP or Blade.
- Patching a Streams component's look from the app.


Checklist: [`checklists/0022-styling.yml`](../checklists/0022-styling.yml)
