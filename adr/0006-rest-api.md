# 0006 REST API

**Status:** Accepted · **Layer:** Streams · Laravel · **Principles:** P1 P3 P6 P7 P8 · **Stack:** 0001

> The API is a second edge onto the same domains: same scope, same actions, explicit routes.

## Rules
1. API code lives in `app/Domains/{Domain}/Api/`, not addons or `routes/api.php`.
2. Every `*ApiResource` is registered in the API service provider. No scanning.
3. Each resource defines `scopedQuery()` aligned with its panel resource; endpoints apply it, never re-filter tenant inline.
4. Endpoints use Streams API entry endpoints and register with `::route($path, $methods)` and explicit verbs.
5. A nested path targets one stream (`/todos/{id}/labels` → `todo_labels`).
6. Writes with business rules dispatch domain actions.
7. Auth is stateless (Sanctum bearer). Tenant comes only from the `X-Organization-ID` header via `API::getTenant()`, never from body, query or session.
8. Queries use explicit three-argument `where($field, '=', $value)`.
9. Each resource has feature tests for tenant scoping and nested routes. *(warning)*

## Shape
```php
final class TodosApiResource extends DomainApiResource
{
    protected static ?string $slug = 'todos';
    protected static ?string $stream = 'todos';

    public static function getEndpoints(): array
    {
        return [
            'index' => ListTodos::route('/', 'get'),
            'show' => ShowTodo::route('/{person}', 'get'),
        ];
    }
}
```
Package reference (query params, relation naming, base classes) belongs in the Streams API docs, not here.

## Not
- Generic `/api/streams/{stream}/entries` as the product API.
- `organization_id` read from the request body.


Checklist: [`checklists/0006-rest-api.yml`](../checklists/0006-rest-api.yml)
