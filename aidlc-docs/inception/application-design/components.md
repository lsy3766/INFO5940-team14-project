# Components

Status: Explicitly approved by the team, recorded at 2026-11-20T19:42:32Z. Documentation only; no implementation or external connection performed. Unit artifacts require separate approval.

## Architecture decisions

Approved choices A/A/A/B/A: capability-centered modules; typed success results with typed errors; dedicated planning coordinator; cohesive persistence gateway owning atomic transactions; final-response request presentation without streaming/background jobs. These are logical components within the approved local stack, not independently deployable services. Units Generation will define work units and code organization after artifact approval.

| ID | Component | Purpose and responsibilities | State and boundaries | Story coverage |
| --- | --- | --- | --- | --- |
| C01 | Local UI/API boundary | Streamlit renders English keyboard-operable forms and waiting states; FastAPI validates typed requests and maps typed errors. Derives confirmation context from explicit user actions, never from model text. | Transient forms and request context; no direct DB/model-key access. | All US-01–US-19 |
| C02 | Preferences | Owns profile preferences, exclusions, diner count 1–10 and meal overrides; defaults 15/45/45 minutes. | Logical profile ownership; persisted through C10. | US-01 |
| C03 | Recipes | Owns catalog, provenance/completeness eligibility, local keyword/ingredient/tag retrieval, source imports and approved variants; maintains 45-record coverage assessment. | Recipe/provenance ownership; C09 handles untrusted fetch, C10 persists. | US-03–US-05, US-18 |
| C04 | Inventory | Exposes versioned stock snapshots and explicit manual add/correct/remove; never hides deduction inside a read. | Logical ingredient stock ownership; all writes through C10. | US-02, US-09, US-11–US-16 |
| C05 | Planning/Shopping | Owns plan draft/save/change/skip, annotations and shopping views; deterministic calculation delegates to C07. Agent revisions require explicit preview/confirmation. | Plan/annotation logical ownership; C10 stores saved plans; drafts are non-authoritative. | US-06–US-10, US-16–US-17 |
| C06 | Cooking Events | Owns preview/confirm/undo application workflows. Validates explicit user intent and coordinates with C10 atomic domain operations; no direct model commit path. | Preview/event lifecycle responsibility; immutable event history and compensations stored by C10. | US-11–US-15, US-17 |
| C07 | Deterministic Policies | Pure typed quantity scaling, compatible-unit conversion, shopping aggregation, exclusion/eligibility/time/repetition validation and high-level transaction/undo checks. No model/DB/network dependency. | No persistent state. | US-01, US-03, US-06–US-09, US-11–US-15, US-18 |
| C08 | Planning Orchestrator | Dedicated bounded model/tool loop for planning and remaining-meal revision. Dispatches allowlisted reads and validation; returns validated proposals, never persists user domain changes. | Ephemeral run state and redacted diagnostic traces, not user confirmation authority. | US-06–US-07, US-16, US-19 |
| C09 | External Adapters | Separate provider adapter for native function calling and recipe-page adapter for bounded public HTTP(S)/JSON-LD parsing. No source terms or model availability assumed verified. | Secrets server-side; fetched content is untrusted data. | US-04–US-05, US-19 |
| C10 | Persistence Gateway | Owns SQLite access and typed domain persistence operations. Atomic confirm/undo couples stock, events, version and idempotency records with rollback. No arbitrary SQL interface exposed to other components. | Physical persistence ownership for profile, recipes, stock, plans, previews, events and request outcomes. | All persistent stories, especially US-12–US-15 |

## Ownership rules

Application services own user intent and use-case coordination; C10 owns persistence/transaction mechanics; C07 owns deterministic policies. “Owns” does not grant an agent permission to write. C01 routes confirmed commands to C02–C06; C08 may read those capabilities and validate proposals but cannot invoke commit/save/undo tools.

C03 publishes eligible recipe records, not raw page instructions. All automatic plans and cooking previews use reviewed complete records. C04 publishes snapshots; no other component independently adjusts stock. C05 can change a plan or annotate takeout/leftovers without consuming ingredients. C06 commits actual cooking once, with explicit confirmation; the amount cooked, not the fraction immediately eaten, determines consumption. C10 alone mutates stock/version/event/idempotency state atomically.

External provider access remains Gemini Developer API gemini-3.5-flash-lite, free-tier only and unverified until later Construction. No general web search, new model fallback, paid service, login, concurrent editing, cloud deployment or leftover portion inventory is introduced.

## Public interfaces

Every component's interface is specified in component-methods.md. Application orchestration is in services.md; allowed calls and trust boundaries are in component-dependency.md. All 19 stories and 33 requirement identifiers are mapped in application-design.md. No omitted UI capability is delegated to an unconstrained chat prompt.
