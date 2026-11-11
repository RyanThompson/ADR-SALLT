# Punch list — ordered by to-dos dogfood

Build against failures in `streams-todo`, not speculation. Prefer `streams:make` + KindRegistry/recipes.

| Priority | Gap | First failure signal | ADR |
|---|---|---|---|
| P0 | Honor `streams.onboard.json` from `streams:onboard` (Composer create-project Path 1) | ✅ `streams:onboard --apply` → OnboardApplier | 0001 |
| P0 | Domain recipe (dir, model, repositories, lang, `streams.json` stream node) | ✅ DomainScaffold (`Todos` kitchen-sink) | 0009 · 0001 |
| P0 | Emit **schema-valid** `streams.json` nodes + `streams:validate` in the loop | ✅ validate after domain emit (source config) | 0001 |
| P0′ | When generators work: regenerate + rebase **RyanThompson/todos** onto golden output | Hand-maintained todos drifts from SDK | — |
| P1 | Domain action cluster (action + DTO + event + test) | Completing/updating a todo has nowhere canon to put behavior | 0010–0012 |
| P1 | Form/modal recipes wired to domain action | UI save path without DTO→action | 0020–0021 |
| P2 | Component kind + `Panel/Components` path | Cards/sections hand-rolled | 0003 |
| P2 | Panel resource registration completeness | Pages not discoverable via resource | 0019 |
| P3 | Policy recipe | Authorize in action without domain policy path | 0014 |
| P3 | Query / listener recipes | Reads and reactions invented | 0010 · 0012 |
| P4 | API resource/endpoints | Second edge missing | 0006 |
| P4 | Alpine kind | Browser-only UX hand-rolled | 0008 |
| P5 | Addon layout align | — | 0007 |
| P5 | Checklist gate + Pest on generate | Canon not enforced by SDK | Gate · 0023 |

See [`RECONCILIATION.md`](RECONCILIATION.md) for the full gap map.
