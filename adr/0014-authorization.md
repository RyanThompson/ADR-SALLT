# 0014 Authorization

**Status:** Proposed · **Layer:** Laravel · **Principles:** P1 P3 P6 · **Stack:** 0001

> Who may act is a domain rule, decided by the owner's policy and enforced by the action.

## Rules
1. Each owned entity has one `{Entity}Policy` in its domain's `Policies/`.
2. Policies are registered explicitly in `AuthServiceProvider::$policies`.
3. Actions enforce with `Gate::forUser($this->data->actor)->authorize(...)`. The action is the authority.
4. Edges mirror the policy for presentation (`->authorize()`, `->disabled()`, `->visible()`). An edge check is never the only check.
5. Policies decide from data. No `return true` placeholders in shipped code.
6. Tenant membership is resolved by the surface's tenant resolver, not by policies. *(warning)*

## Shape
```php
final class TodoPolicy
{
    public function update(Identity $actor, Todo $todo): bool
    {
        return $actor->owns($todo) || $actor->administers($todo->organization_id);
    }
}

// Action (authority)
Gate::forUser($this->data->actor)->authorize('update', $todo);

// UI edge (presentation)
Action::make('rename')->authorize('update', $todo);
```

## Not
- Authorization only in a Livewire page or modal.
- `auth()->user()->can()` inside an action.


Checklist: [`checklists/0014-authorization.yml`](../checklists/0014-authorization.yml)
