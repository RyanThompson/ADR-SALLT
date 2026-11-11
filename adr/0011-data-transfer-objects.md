# 0011 Data Transfer Objects

**Status:** Accepted · **Layer:** Laravel · **Principles:** P3 P4 · **Stack:** 0001

> Input is a typed, immutable DTO built at the edge. Nothing is guessed.

## Rules
1. DTOs are `final readonly class` with typed, constructor-promoted properties.
2. Names end in `Data` and match their consumer (`UpdateTodoTitle` → `UpdateTodoTitleData`).
3. Context travels in the DTO: actor, tenant, time and source.
4. Named factories (`fromForm`, `fromValidated`, `fromApi`) map trusted input with no hidden defaults and no runtime globals.
5. Required fields are required. Nullable only when optional by design.
6. DTOs hold data only — no persistence, dispatch, authorization or branching.

## Shape
```php
final readonly class UpdateTodoTitleData
{
    public function __construct(
        public int $todoId,
        public string $title,
        public Identity $actor,
        public int $organizationId,
        public CarbonImmutable $at,
    ) {}

    public static function fromForm(array $validated, int $todoId, Identity $actor, int $organizationId, CarbonImmutable $at): self
    {
        return new self($todoId, $validated['title'], $actor, $organizationId, $at);
    }
}
```

## Not
- `$validated['title'] ?? ''` for a required field.
- Factories that call `auth()` or `now()`.
- Array-shape docblocks instead of a DTO.


Checklist: [`checklists/0011-data-transfer-objects.yml`](../checklists/0011-data-transfer-objects.yml)
