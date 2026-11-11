# 0009 Domains

**Status:** Accepted · **Layer:** Laravel · **Principles:** P1 P2 P5 · **Stack:** 0001

> Code is organized by business capability. Each domain owns its data, behavior and surfaces.

## Rules
1. Business-capability code lives in `app/Domains/{Domain}`. Only framework glue and truly domain-neutral infrastructure live outside.
2. Domains are plural business nouns (`Todos`, `Projects`, `Labels`). Never technical buckets (`Data`, `Services`, `Helpers`, `Managers`).
3. A domain owns its streams, actions, DTOs, events, listeners, policies, API and panel UI.
4. Derived domains (insights, reports, dashboards) read facts and never write them.
5. Prefer one coherent domain over several fragments with unclear ownership. *(warning)*

## Shape
```text
app/Domains/{Domain}/
├── Actions/      commands + their *Data DTOs (+ *Result when a command returns a value object)
├── Queries/      named reads (no events)
├── Events/       past-tense facts
├── Listeners/    reactions owned by this domain
├── Policies/     authorization per owned entity
├── Contracts/    public interfaces other domains call
├── Models/       entry/model classes (Streams Entry)
├── Repositories/ Streams repository classes wired from the stream definition
├── Support/      domain-private helpers
├── Api/          Resources/, Endpoints/
└── Panel/        Resources/, Pages/, Components/, Forms/, Tables/, Actions/
```
Create a folder only when it has a file. Stream definitions hook `config` (model, repository, source) into `streams.json` / `streams/{id}.json` and must pass `streams:validate` (0001).

## Not
- Business logic in `app/Services`, `app/Helpers` or controllers.
- A model two domains both write.
- A reporting domain that "fixes" source data.


Checklist: [`checklists/0009-domains.yml`](../checklists/0009-domains.yml)
