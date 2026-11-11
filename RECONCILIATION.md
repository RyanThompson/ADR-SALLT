# SDK reconciliation — gap map

**Status:** Landed (Phase 1). This is the ordered backlog before trusting implementation.  
**Sources:** ADR-SALLT canon · `streams/sdk` @ local `_packages/streams-sdk` (`563babf`, path-pinned from `streams-todo`) · GV2 domain plumbing as architecture reference only · dogfood app `streams-todo`.

**Dogfood note:** `streams/sdk` `composer.json` allows `nikic/php-parser` `^4.14 || ^5.0` so Laravel 12 apps can install the path package.

Canon “good” is defined by ADRs + checklists. Emit “can” is defined by `streams:make` + KindRegistry + tree/validate. Gaps below are the difference.

---

## 1. What exists today

| Capability | Reality |
|---|---|
| App tree | `streams.json` + `$ref` files; draft-07 `app.schema.json` + per-node schemas |
| Validate | `streams:validate` (+ stream definition schema); self-describing `$schema` |
| Make/build | **`streams:make` only** — edit tree, then generate PHP; dry-run, lock, rename, menu, recipes |
| Capture | `streams:capture` — panel builders → tree (opaque nodes for closures) |
| Kinds | Built-ins: `stream`, `page`, `table`, `action`, `manage` via `KindRegistry` |
| UI paths | `app/Domains/{Domain}/Panel/{Tables,Pages,Actions,Resources,Forms}` (+ Public) |
| Extensibility seed | `registerFromSdk` / `App` / `Package` + `config('streams.sdk.make.kinds')` |
| Legacy cmds | `make:stream`, `make:entry`, `make:addon`, `make:table` — classes exist, **not** primary surface |
| MCP (optional) | Agent tools when `laravel/mcp` present |

---

## 2. Missing generators / kinds / recipes

Ordered by dogfood likelihood (to-dos hits first). Status vs `streams:make` + KindRegistry — not a fictional `make:*` catalog.

| # | Artifact | How it should appear | ADR | SDK today | Why |
|---|---|---|---|---|---|
| 1 | **Domain recipe** | Domain dir + lang + model + repositories + validated stream node in `streams.json` | 0009 · 0001 | ❌ | Entry point; without it agents invent folders |
| 2 | **Model + Repository + stream hookup** | `Models/{Entity}`, `Repositories/{Stream}`, stream `config` wiring | 0009 | ❌ | Streams core expects this; ADR 0009 still under-specifies `Repositories/` |
| 3 | **`streams.json` typed workflow** | Generators write schema-valid nodes; validate in CI/agent loop | 0001 | ⚠️ validate exists; domain emit does not | Self-checking tree is useless if scaffolds write invalid JSON |
| 4 | **Domain action cluster** | Action + `*Data` + Event (+ Pest) as one recipe | 0010–0012 | ❌ | Highest-leverage domain plumbing; closes UI→DTO→action |
| 5 | **Form/modal → domain action** | UI kinds wired to action recipe | 0020–0021 | ⚠️ UI only | Tree builds form/action UI; no domain Action/`*Data` |
| 6 | **Component kind** | `Panel/Components/` + path template | 0003 | ❌ | Composition cards/sections; path missing from SDK defaults |
| 7 | **Panel resource completeness** | `getPages()` / navigation registration | 0019 | ⚠️ | Path exists; registration not first-class |
| 8 | **Policy + `$policies`** | Domain recipe | 0014 | ❌ | Authz in the action path |
| 9 | **Query / Listener** | Domain recipes | 0010 · 0012 | ❌ | Reads and reactions |
| 10 | **API resource + endpoints** | Kind/recipe | 0006 | ❌ | Second edge |
| 11 | **Alpine component + JS entry** | Kind/skill | 0008 | ❌ | Headless / browser layer |
| 12 | **Addon tier layout** | Align scaffold with ADR 0007 | 0007 | ⚠️ | Class exists; layout drift |
| 13 | **Checklist gate on output** | Generated files pass checklist `error`s | Gate | ❌ | “SDK enforces ADRs” |
| 14 | **Pest stubs on build** | With each recipe | 0023 (open) | ❌ | Proof |

**Stubs present, no SDK emit yet:** action, data, event, action-test, query, listener, policy, page, form, table, modal, component, api-resource, alpine, lang.  
**Stubs missing:** domain root, model, repository, panel-resource, stream/tree recipe, addon tier layout.

---

## 3. Scalability & maintainability audit

**Strengths**
- One build command + lock + dry-run + opaque capture scales better than N Artisan commands.
- KindRegistry separates “what can be made” from Artisan UX.
- JSON Schema + validate = self-checking tree; good agent surface.
- Path templates configurable (`streams.sdk.make.paths`).

**Risks**
- People-slice assumptions in renderers / built-in kinds (`actions.add`, manage chain) — must generalize for arbitrary domains (e.g. `todos`).
- Dual mental model: old canon `make:*` lists vs real `streams:make` (docs now corrected; keep them honest).
- UI-only generation leaves domain layer to LLMs — opposite of “prefer SDK over tokens.”
- No golden checklist CI — stubs, ADRs, and renderer will drift.
- Capture opaque nodes grow if builders hold logic — keep builders data-like.
- Unregistered legacy `make:*` confuse discovery.
- ADR 0009 omits `Repositories/` while real Streams apps need them — generators will disagree with canon until fixed.

**Recommendation:** one fixture (to-dos), parameterized renderer, stubs as golden tests, checklist YAML as acceptance harness for `streams:make` output.

---

## 4. Extensibility — registry design (open)

**Already in SDK:** `KindRegistry` (sdk / app / package). Right spine for UI/stream nodes.

**Proposed open model (not implemented):**

```
GeneratorRegistry  (or SkillRegistry)
├── kinds[]       — what can be defined (extends KindRegistry)
├── builders[]    — kind → files (renderer / stubs / transforms)
├── recipes[]     — multi-kind chains (domain+stream+model+repo+manage)
├── validators[]  — beyond JSON Schema (checklist ADR rules)
└── skills[]      — agent wrappers: intent → recipe → streams:make → checklist
```

**Registration surfaces (open):** PHP provider API · `streams.sdk.make.recipes` / `skills` config · optional `salty.generators.json` · Cursor/MCP skills that only call registered recipes.

**Leave open:** domain Actions as tree nodes vs side recipes; MCP vs Cursor skills vs both; naming.

---

## 5. Foundational before code

1. ADR 0001 sole stack home (Phase 2) — done when Part 2 leaves 000.3.
2. ADR 0009: add `Repositories/`, document stream JSON model/repo hooks.
3. Retarget all generation docs to kinds/recipes (done in `generate/README.md`).
4. Pin to-dos dogfood (Phase 3); build against real failures ([`PUNCHLIST.md`](PUNCHLIST.md)).
