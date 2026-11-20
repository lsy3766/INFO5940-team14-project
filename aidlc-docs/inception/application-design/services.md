# Services and Orchestration

Status: Explicitly approved by the team, recorded at 2026-11-20T19:42:32Z. Documentation only; no implementation or external connection performed. Unit artifacts require separate approval.

## Service map

| Service role | Component | Coordinates | Does not authorize |
| --- | --- | --- | --- |
| Profile application service | C02 | Read/save preferences and overrides through C07/C10 | Extra profiles, login or model writes |
| Recipe application service | C03 | Search, import preview, missing-data correction, reviewed save/variant approval through C09/C07/C10 | General web search, fabricated provenance, unconfirmed agent saves |
| Inventory application service | C04 | Versioned snapshots and explicit manual edits through C07/C10 | Implicit consumption during planning |
| Plan/shopping application service | C05 | Draft change, explicit save, annotations and deterministic shopping via C07/C10 | Raw-stock deduction or automatic revision save |
| Cooking application service | C06 | Preview, explicit confirmation, undo; calls C10 atomic operations | Model-generated permission or direct SQLite access |
| Planning orchestration service | C08 | Bounded model loop, eligible retrieval, stock/preferences reads and deterministic validation | Save, deduction, undo or external-source permission approval |
| Persistence gateway | C10 | Typed reads/writes and atomic stock/event/request outcomes | Unvalidated model or UI direct write paths |

All service names describe logical application roles, not network-deployed services.

## Planning and revision flow

1. C01 validates the user request and calls C08. C08 reads C02 preferences and C04 stock; C03 retrieves eligible attributed candidates with C07 filters. Model context includes selected recipe source content, not a claim of model-internal recipe verification.
2. C08 invokes the C09 provider adapter with typed tool schemas, request deadline and selected context. Model tool requests are untrusted. C08 checks names/arguments before calling deterministic read/compute interfaces.
3. Exposed tool capabilities: read preferences, read inventory, retrieve/get eligible recipes, scale dishes, aggregate shopping requirements and validate proposed plans. Invalid calls return typed tool failures. There is no save/commit/undo/import-write tool in this planning allowlist. A suggested public URL can be handed back for the separate recipe workflow; it is not automatically fetched or persisted by model authority.
4. C07 validates 15 slots, eligibility, diner bounds, exclusions, time and repetition rules. Tool results feed subsequent bounded revision when needed. If infeasible, report it or ask for a specific soft relaxation; no silent exception. Transport/system/tool prompts cannot count as user permission.
5. C08 returns a validated proposal, warnings/shortages and a redacted trace. C05 saves only after explicit user action bound to the proposal. C08 itself performs no persistent user-domain write.
6. After confirmed cooking/undo/manual stock changes, C08 may re-evaluate remaining meals using current stock. It returns a new proposal; C05 does not overwrite saved meals until confirmed. Saved data can be read again on subsequent user actions; no background worker is introduced.

## Recipe import and variant flow

C01 sends a requested public URL or pasted source text to C03. C09 validates supported/public destinations and bounded fetches/parsing; text remains untrusted. C03 checks provenance/use eligibility, required fields, units/base servings and review status. Missing information is displayed, not invented. Confirmed corrected complete records are saved by C10; incomplete/unreviewed records are excluded from automatic use. Substitution variants preserve source lineage, require explicit review/approval and fresh exclusion/quantity checks. The 45 eligible-record floor and Chinese/Western/all-meal coverage are reported, not claimed already achieved. Dataset acquisition and source-use checks happen in later Construction.

## Cooking preview and commit flow

C06 accepts an eligible recipe from library or plan, serving count and corrected actual use. It obtains current C04 snapshot and C07 validation and returns quantities, shortages and prospective balances. Creating technical preview state through C10 must not change inventory or record completed cooking. Cancel/view is non-consuming. Confirmation is a separate C01 user action bound to preview identity, revision/content, expected inventory version and request identifier.

C06 checks that action context and passes an authorized typed command to C10. C10's atomic transaction first handles identical recorded replay or conflicting identifier reuse, resolves the reviewed preview and current authoritative state, and revalidates version/stock/recipe data using deterministic policies. A stale or insufficient preview produces no deduction. Success commits stock, cooking event, version and idempotent outcome together; failure rolls back all partial effects. A UI click alone cannot override invalid stock, and a model message cannot create a confirmation context.

If an HTTP response is lost, display an uncertain result rather than a success claim; look up/replay the same authorized request identifier to retrieve the recorded outcome. Do not issue a fresh identifier blindly and thereby create a duplicate event. Exact implementation details belong to Functional Design/Construction.

## Undo and failure recovery

C06 reads the original event/current state, presents compensation and requires explicit confirmation. C10 checks current safety and whether the event was already undone, atomically restores the valid quantities and appends a compensation with recorded request outcome. No original event deletion or second restoration. Unsafe requests fail without mutation; precise later-change safety predicates are deferred to Functional Design rather than guessed as new product scope.

For stale/insufficient stock, the user corrects manual stock or actual quantities and requests a fresh preview before a new valid confirmation. Manual direct Save/Add/Edit/Delete confirms only that manual edit. Takeout/leftover annotations persist via C05 without raw-stock effects; original cooking is deducted once.

## Presentation and bounded errors

C01 shows a waiting state and final typed result/error; no token/status streaming or background jobs. Model requests terminate within 60 seconds, with clear user-controlled retry; non-model local actions target 2 seconds. Errors are centrally mapped to safe messages. C08 records actual tool outcomes rather than invented success; no paid provider/search fallback or automatic billing. Diagnostic traces redact secrets and unnecessary user data. Future evaluation inspects retrieval, inventory reads, deterministic validation and revision on 10 fixed scenarios: at least 8 valid plans/correct infeasibility outcomes; all 10 preserve exclusions/confirmation; feasible saved plan within one requested revision.
