# Unit of Work Definition

Status: Explicitly approved by the team, recorded at 2026-11-20T19:58:21Z. Units Generation and Inception are complete. STOP before Construction. Documentation only; no application or test files created.

## UOW-01 — Local Cooking Assistant

One whole-application planning unit contains all US-01–US-19, both illustrative usage-context personas, all 16 FR, nine NFR and eight acceptance requirements, and Q-01–Q-06. Internal modules implement logical responsibility boundaries, not independent deployments or per-person units. Separate Streamlit/FastAPI entry points remain parts of the same local application scope. Existing external model/data sources are dependencies, not team-owned work units.

Approved decisions: unit plan A/A/A/A and Application Design A/A/A/B/A. Capability-centered organization, typed success/central typed errors, dedicated planning orchestration, gateway-owned atomic transactions and bounded final responses remain unchanged.

### Inclusions

One local profile/inventory without login or concurrent editing; 1–10 diners with shared dishes and meal overrides; five days/15 slots; 45 eligible complete distinct Chinese/Western recipes across all meals; local keyword/ingredient/tag RAG and supported public-URL imports; manual inventory; deterministic scaling/shopping; flexible plan edits; actual-cooking preview/confirmation; stale/shortage/duplicate recovery; safe once-only undo; remaining-meal proposals; annotation-only takeout/leftovers; inherited safety, accessibility, timing, agent evaluation and PBT obligations.

### Exclusions and feasibility

No original recipe authoring requirement, general live search, separate profiles, independent service scaling, cloud hosting, payment/pricing, cookware matching, image recognition, medical nutrition advice, leftover portion inventory or paid dependency. Dataset/source permissions, 45-record corpus availability, free Gemini access/tool quotas, exact releases, runtime performance and detailed algorithms remain future Construction checkpoints. No access, downloads, tests or implementation happen here; a failed checkpoint must be reported rather than silently weakening scope.

## Module catalog and code organization strategy

All paths in this document are planned future repository-relative locations, not existing files created by this stage. Application code belongs at repository root, never inside aidlc-docs/. Supporting modules do not become primary owners of user business outcomes.

| Module | Name | Design component | Responsibility | Future path |
| --- | --- | --- | --- | --- |
| M01 | Preferences | C02 | Profile, exclusions, diner/time overrides | app/preferences/ |
| M02 | Recipes | C03 | Catalog, provenance/eligibility, local retrieval, reviewed imports and variants | app/recipes/ |
| M03 | Inventory | C04 | Versioned snapshots and explicit manual stock edits | app/inventory/ |
| M04 | Planning/Shopping | C05 | Plan drafts/save/change/skip, shopping views, annotations; business responsibility for planning outcomes | app/planning/ |
| M05 | Cooking Events | C06 | Actual-consumption preview, explicit confirmation, safe undo workflows | app/cooking/ |
| M06 | UI/API | C01 | Streamlit forms/waiting/final views; FastAPI typed boundary and user-action confirmation context | app/ui.py; app/api.py |
| M07 | Deterministic Policies | C07 | Pure quantity/unit, eligibility/constraint and stock/undo validation policies | app/policies/ |
| M08 | Planning Orchestration | C08 | Bounded planning/revision model loop, allowlisted read/compute tools and redacted traces | app/orchestration/ |
| M09 | External Adapters | C09 | Separate provider and safe public recipe-fetch/parser adapters | app/adapters/ |
| M10 | Persistence Gateway | C10 | Typed SQLite reads/writes; sole physical stock/event/version/idempotency transaction owner | app/persistence/ |

Additional planned locations: app/contracts/ is the single authoritative typed domain/interface catalog; tests/ contains capability test areas and a separate tests/integration/ area; reusable future property strategies belong under tests/ shared helpers rather than duplicated copies. Runtime SQLite data and secrets belong in ignored local locations configured later, not committed fixtures with real profiles/keys. Exact packaging, schemas and filenames beyond these strategy paths remain later implementation details. No production/test directory is created now.

Contract definitions in app/contracts/ must not import implementation modules; pure M07 depends on contract types only, not persistence/model/network. Capability modules depend on the gateway interface, not SQLite directly. Gateway internals own repositories/transactions; no parallel repository abstraction is imposed on capability services contrary to approved Q4 B. Provider and public-page adapters remain separate responsibilities within M09.

## Interface ownership and collaboration

The approved component-methods.md is the initial authoritative design contract catalog, to be realized later in app/contracts/. Do not fork incompatible type definitions inside modules. Capability owners jointly review changes to their requests/results; affected consumers review compatibility. Existing role leads coordinate shared reviews, without exclusive implementation ownership: Shuyue Li (workflow/GitHub), Shining Cao (product/requirements), Danying Wei (UX/stories), Yuyi Long (technical/LLM), Yichen Tong (evaluation/delivery). These are supplied responsibilities, not inferred module allocations or authority to approve for absent teammates. Future coding assignments need a separate team decision.

## Non-negotiable state and authority boundaries

- M06 derives explicit action-specific user intent; relevant capability service validates it. Model text, source pages and diagnostic traces cannot create confirmation.
- M08 returns proposals through read/compute tools only; it cannot call save, commit or undo. M04 saves confirmed plan revisions without stock deduction. No automatic plan-save or stock reservation.
- M05 owns cooking/undo workflow responsibility; M10 alone owns physical atomicity. Stock, event/compensation, inventory version and idempotent recorded outcome commit together or not at all. Manual stock edits also use M10 and never invent cooking events.
- Stale/insufficient previews reject with no mutation; correction requires a fresh valid preview. Identical successful replay returns recorded outcome, while conflicting request reuse is rejected. Undo preserves original history and cannot restore twice.
- No remote call is held inside a stock transaction. M07 checks authoritative state within M10 operations. Takeout/leftover annotations never double-deduct original raw ingredients.
- M02 admits only eligible complete reviewed records into automatic plans/deductions. M09 supplies untrusted content; source terms/provenance are not inferred from availability. Units do not introduce source providers or unverified datasets as selected dependencies.

## Delivery and verification responsibility

UOW-01 is accountable for all future acceptance; primary story modules coordinate user outcomes, not isolated independent releases. M07/M10 support arithmetic, round-trip, invariant, idempotency and stateful Hypothesis verification; critical example tests remain required. Runtime checks are future work: local actions target 2 seconds, model requests time out within 60 seconds, English keyboard operation; 10 fixed agent scenarios with at least 8 valid-plan/correct-infeasibility successes, all 10 preserving exclusions/confirmation, trace inspection and feasible save within one requested revision. No test results or implementation compliance are claimed.

Detailed precision/rounding, ingredient identity/duplicate detection, multi-dish time interpretation, preview lifetime/binding mechanics, safe-undo predicates, schemas and loop/fetch caps remain listed in approved application-design.md for future Functional Design; material product tradeoffs must return to the team. No Construction work is started by mapping these responsibilities.

## Approval gate

Review all three unit artifacts. Request changes or explicitly approve them. All three artifacts are now explicitly approved; Units Generation and Inception are complete. STOP; no Construction, application code or test files.
