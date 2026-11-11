# 0007 Addons

**Status:** Accepted · **Layer:** Laravel · Streams · **Principles:** P1 P6 · **Stack:** 0001

> An addon is a Composer package that registers capabilities. It never owns core.

## Rules
1. Addons live in `addons/available/{addon}`, `addons/private/{client}/{addon}` or `addons/internal/{addon}`. Create a tier when its first addon exists.
2. Each addon has its own `composer.json`, PSR-4 `src/`, and one service provider as its entry point.
3. The root `composer.json` declares a path repository per tier in use.
4. Visibility follows tier: `internal` is never listed; `private` is assignable only to its client.
5. Capabilities register through the provider. No runtime scanning.
6. Addons do not own core streams or REST for core streams.

## Shape
```text
addons/
  available/{addon}/composer.json, src/{Addon}ServiceProvider.php
  private/{client}/{addon}/
  internal/{addon}/
```

## Not
- Client-specific logic in app domains.
- Visibility flags that ignore tier.


Checklist: [`checklists/0007-addons.yml`](../checklists/0007-addons.yml)
