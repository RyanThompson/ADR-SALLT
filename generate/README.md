# Generation

The Streams SDK should generate canon. Generated output must pass every `error` rule in `checklists/` with no edits except filling TODOs. The stubs in [`stubs/`](stubs/) are the golden target for missing kinds.

Architecture lens: [`adr/0001`](../adr/0001-salty-stack.md). Business-only decimals: [`000.0`](../000.0-mission.md)–[`000.3`](../000.3-foundations.md).

## How the SDK actually works

There is **one data artifact** and **one primary build command**. Do not invent a parallel `make:page` / `make:action` / `make:form` Artisan catalog for Streams UI — that is not how `streams/sdk` works.

| Piece | What it is |
|---|---|
| **App tree** | `streams.json` plus `$ref` node files. Dot paths address nodes (`todos`, `todos.table`, `todos.pages.manage`, `todos.actions.add`). |
| **Schemas** | Self-describing JSON Schema (`app.schema.json`, per-node schemas, stream definition schema). The tree and definitions point at `$schema`. |
| **`streams:validate`** | Validates stream definitions (and related checks) against schema and the running app. Prefer `--json` for agents. |
| **`streams:make`** | **The** generation command. Define or edit a node (interactive prompts, flags, `--json=recipe.json`, `--menu`, `rename`), then build PHP from the tree. `--dry-run` plans without writing. Lock file tracks managed output. |
| **`streams:capture`** | Reads existing `app/Domains/*/Panel/...` builder settings into the tree. Closures and non-literal settings become opaque nodes make will not rewrite. |
| **`KindRegistry`** | Kinds are registry data, not Artisan commands. Built-ins today: `stream`, `page`, `table`, `action`, `manage`. Apps/packages register more via `registerFromApp` / `registerFromPackage` or `config('streams.sdk.make.kinds')`. |
| **`streams:schema`** | Emits entry schemas and the app-definition schema set used to validate the tree. |

```bash
# Typical agent / CI shape
php artisan streams:validate --json
php artisan streams:make --list --json
php artisan streams:make todos.actions.add --json=recipe.json --no-interaction
php artisan streams:make --dry-run --json
php artisan streams:capture --tree=streams.json
```

Legacy command **classes** (`make:stream`, `make:entry`, `make:addon`, `make:table`) may exist in the package for findability. They are **not registered** as the primary surface; `php artisan list` shows `streams:make`, not a per-artifact `make:*` list from the SDK.

## Contract (canon the SDK must satisfy)
1. **Kinds, not a `make:*` zoo.** Each artifact kind is a KindRegistry entry (or a recipe chaining kinds). The name/handle is the input; path, suffix and namespace come from config (`streams.sdk.make.paths`) and P2.
2. **Tree first, then PHP.** Authoring writes validated JSON into `streams.json`; building emits files. Re-runs are idempotent: unchanged managed files skip; conflicts report, they do not silently overwrite hand edits.
3. **Siblings come together.** A recipe creates everything the canon requires with it (e.g. a domain action brings its DTO and event). Prefer chained kinds / recipes over orphan files.
4. **Register what you create.** Emit or print registration (provider, `getPages()`, `getEndpoints()`, `$policies`, JS entry) — nothing relies on discovery (P6).
5. **Copy is keys.** User-facing strings are translation keys in `lang/{locale}/domains/{domain}.php` (0015).
6. **A test comes with it.** Each artifact gets a Pest stub proving canon behavior where the kind owns behavior.
7. **Bases are configurable.** Projects substitute bases (e.g. to-dos bench `App\Livewire\PanelPage`, `DomainApiResource`) through SDK config.
8. **Checklist gate.** Generated output must pass every `error` rule in the governing checklists.

## Coverage (kinds / recipes vs today)

Status is against the live `streams:make` + KindRegistry model — not a fictional `make:*` list.

| Artifact | How it should appear | Creates | ADR | SDK today |
|---|---|---|---|---|
| Stream | `stream` kind / tree node | Stream node in `streams.json` (+ definition) | 0001 · 000.3 | ✅ |
| Entry | entry tooling (`make:entry` class / MCP) | Entry row | — | ✅ (not the UI build path) |
| Manage slice | `manage` chain | Page + table + modal add action | 0001 · 0019–0021 | ✅ built-in chain |
| Page | `page` kind | `Panel/Pages/{Class}.php` | 0019 | ⚠️ |
| Table | `table` kind | `Panel/Tables/{Class}.php` | 0004 | ⚠️ |
| Action / modal | `action` kind | `Panel/Actions/{Class}.php` | 0021 | ⚠️ |
| Form | form node under action / panel | `Panel/Forms/{Class}.php` | 0020 | ⚠️ partial |
| Panel resource | panel path type | `Panel/Resources/{Class}.php` | 0019 | ⚠️ partial |
| Addon | addon scaffold (legacy class / future kind) | Tiered package + provider + path repo | 0007 | ⚠️ must match ADR 0007 layout |
| Domain | **domain kind / recipe** | Domain dir + lang + model + repositories + `streams.json` hookup | 0009 | ❌ |
| Model / Repository | with domain recipe | `Models/{Entity}.php`, `Repositories/{Stream}.php`, stream config | 0009 | ❌ |
| Domain action cluster | **recipe** (not a UI kind alone) | `Actions/{VerbNoun}.php`, `*Data.php`, `Events/{Event}.php`, test | 0010–0012 | ❌ |
| Query | domain recipe | `Queries/{Name}.php`, test | 0010 | ❌ |
| Listener | domain recipe | `Listeners/{Name}.php` + registration | 0012 | ❌ |
| Policy | domain recipe | `Policies/{Entity}Policy.php` + `$policies` | 0014 | ❌ |
| Component | **component kind** + path template | `Panel/Components/{Name}.php` | 0003 | ❌ (no kind; path not in defaults) |
| Alpine | alpine kind / skill | `resources/js/components/{name}.js` + entry registration | 0008 | ❌ |
| API resource | api kind / recipe | `Api/Resources/*`, `Api/Endpoints/*`, provider | 0006 | ❌ |

## Order of build (proposed — not implemented here)
1. **Domain recipe** — directory, model, repositories, validated `streams.json` stream node, lang.
2. **Domain action cluster** — action + DTO + event (+ test); highest-leverage non-UI gap.
3. **Form / modal recipes** wired to that action — closes UI → DTO → action.
4. **Component kind** + path; complete panel/page/table registration.
5. Policy, listener, query recipes.
6. API + Alpine kinds.
7. Align addon scaffold with ADR 0007.

Extend via **KindRegistry** (and future recipe/skill registration) — apps register their own kinds so product workflow is codified without forking the SDK. See the reconciliation report for the open extensibility design.

## Stubs
[`stubs/`](stubs/) are the golden PHP/JS shapes when a kind or recipe does not exist yet. Agents copy a stub only if the SDK cannot emit the artifact; once a kind lands, the stub is the fixture the renderer must match.

## Placeholders (stubs)
`{{ namespace }}` `{{ class }}` `{{ domain }}` (Todos) `{{ domainKey }}` (todos) `{{ stream }}` `{{ entity }}` (Todo) `{{ entityVar }}` (todo) `{{ action }}` `{{ data }}` `{{ event }}` `{{ name }}` (kebab registry name)
