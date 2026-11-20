# Application Design Plan

Status: Plan explicitly approved by the team, recorded at 2026-11-20T19:40:01Z. Five design artifacts generated and explicitly approved by the team. Choices A/A/A/B/A preserved.

## 1. Approved baseline and design scope

References: requirements/requirements.md (FR-01–FR-16, NFR-01–NFR-09, AC-01–AC-08), user-stories/stories.md (US-01–US-19, 52 scenarios and Q-01–Q-06), user-stories/personas.md (P-01/P-02), and plans/execution-plan.md, all under inception/.

Comprehensive high-level Application Design will identify components, responsibilities, typed interfaces, application-service orchestration, dependency relationships and data flows. Detailed algorithms, database schemas/migrations, implementation, test code, dependency installation and provider/source feasibility tests are deferred to later Construction. No model or external service connection is performed now.

Settled constraints are not reopened: Python/FastAPI/Pydantic, Streamlit, SQLite, direct provider SDK/small explicit tool loop, one local profile and inventory without login/concurrent editing, five days/15 slots, 1–10 diners, 45 eligible complete distinct existing-source recipes, local RAG plus supported URLs, no general search, deterministic calculation/validation, preview and explicit cooking confirmation, exactly-once deduction, safe once-only undo, annotation-only leftovers, free-tier-only model subject to later checks, and all approved timing/evaluation/PBT targets.

The questions below concern responsibility boundaries and high-level contracts, not new product scope. Logical services mean orchestration roles; they do not imply independently deployed microservices. Deployment-unit selection and code organization will be finalized in Units Generation after design approval.

## 2. Capabilities to allocate after choices are approved

- Preferences and meal overrides; manually maintained inventory and versioned snapshots.
- Recipe catalog, eligibility/provenance review, public-URL retrieval/parsing and pasted-source recovery.
- Deterministic serving/unit arithmetic, shopping aggregation and plan constraint validation.
- Planning and revision orchestration with explicit model tools and honest failure handling.
- Plan save/confirmation, cooking preview/commit, idempotency, undo and remaining-meal review.
- Takeout/leftover annotations without portion inventory or repeat deduction.
- UI/API boundary, persistence adapters, provider adapter and verification/trace responsibilities.

This list is a coverage checklist, not a finalized component inventory or implementation task list. Assign every capability and approved cross-cutting obligation during design; preserve source and stock integrity boundaries.

## 3. Execution checklist

### Planning and gates

- [x] Load Application Design rules and approved requirements/story/execution context.
- [x] Record execution-plan approval and identify unsettled design decisions.
- [x] Create this plan with mandatory artifacts, checkboxes and embedded questions covering all design categories.
- [x] Collect all five answers without choosing for the team.
- [x] Validate completeness, compatibility with approved scope, and mutual consistency.
- [x] Check need for follow-up clarification: none required; no substantive ambiguity found.
- [x] Incorporate validated choices and log the complete design-plan approval prompt.
- [x] Obtain explicit plan approval before artifact generation.

### Generation after plan approval

- [x] Re-read the approved plan/context and identify the next incomplete generation step.
- [x] Generate inception/application-design/components.md: names, purpose, responsibilities, public interfaces, owned state and boundaries; map approved stories/requirements.
- [x] Generate inception/application-design/component-methods.md: method signatures, typed input/output contracts, high-level purpose, confirmation/freshness/idempotency expectations and error categories. Do not implement methods or detailed business rules.
- [x] Generate inception/application-design/services.md: service responsibilities and orchestration for planning/revision, recipe import/review, cooking confirmation/undo and remaining-meal proposals; separate deterministic enforcement from model proposals.
- [x] Generate inception/application-design/component-dependency.md: dependency matrix, communication patterns and data-flow diagrams with text alternatives; show model, untrusted web content, user confirmation and persistence trust boundaries.
- [x] Generate inception/application-design/application-design.md consolidating the four design documents and recording chosen alternatives, traceability and deferred detailed-design questions.
- [x] Validate completeness across all 19 stories, 16 FR, 9 NFR and 8 AC identifiers; verify no extra accounts, live search, paid dependency, code execution, cloud hosting or new MVP feature is introduced.
- [x] Validate preview-without-mutation, exclusion checks, source eligibility, typed quantity handling, backend-enforced confirmation, atomic stock writes, idempotency and safe undo ownership; retain quantitative timing and agent evaluation obligations.
- [x] Review extension applicability; validate diagrams/Markdown before writing; immediately update task/state checkboxes for completed steps.
- [x] Log and present the complete design artifact review prompt; wait for explicit artifact approval before Units Generation.

### Separate approval tracking

- [x] Team explicitly approves the finalized design plan.
- [x] Team explicitly approves all generated Application Design artifacts.

## 4. Design questions — answer all five

Use a letter after each [Answer]:; X allows a custom choice with clear rules. All choices must preserve the approved requirements. Submitting answers is not plan approval. No source site, live service, dependency release or code ownership decision is requested here.

### Question 1: Logical component organization

How should domain responsibilities be grouped within the local Python backend? Both alternatives keep Streamlit calling FastAPI and prevent UI access to SQLite or the model key.

A) Capability-centered logical modules: Preferences, Recipes (with import/retrieval adapters), Inventory, Planning/Shopping, Cooking Events, plus shared quantity/constraint policies. UI and external adapters depend on these boundaries through application services. This makes business ownership visible while retaining one local backend.

B) Layer-centered organization: Presentation/API, Application Services, Domain Policies and Persistence/External Adapters. Label recipe/inventory/plan/cooking responsibilities inside those layers, with explicit state ownership to prevent a generic service from silently writing across domains.

X) Other (describe logical boundaries within the approved local stack after [Answer]:).

[Answer]: A

### Question 2: Method and error contract style

Which high-level interface contract should components expose? Both alternatives use validated typed requests/results, explicit units, preview/version/request identifiers where relevant, and clear failures rather than ambiguous success strings.

A) Successful calls return typed result objects; validation, not-found, unsupported-source, infeasible, stale-stock, insufficient-stock, duplicate/conflicting request and provider-unavailable conditions use documented typed errors mapped centrally at the API/tool boundary. Replayed successful confirmations return their recorded result rather than a second effect.

B) Every call returns an explicit typed success-or-failure result envelope with the same documented error categories; application services and tool adapters must inspect the outcome before continuing. Replayed successful confirmations return the recorded success without a second effect.

X) Other (specify equally explicit typed outcome/error rules after [Answer]:).

[Answer]: A

### Question 3: Agent and application-service orchestration

Where should the model/tool loop coordinate planning and revision? Both options keep deterministic validation and persistent write authorization outside the model, and require user confirmation for agent-proposed writes.

A) A dedicated planning orchestration service owns the bounded model/tool loop and calls inventory, recipe and deterministic validation interfaces. Cooking confirmation, undo and import persistence have separate application services; the model can propose actions but cannot directly commit stock.

B) A general agent coordinator owns the bounded loop for planning, recipe assistance and remaining-meal revision, routing through capability-specific application services. Write-capable services still require backend-validated user confirmation and cannot be bypassed by the coordinator.

X) Other (describe who coordinates the loop and how deterministic services enforce boundaries after [Answer]:).

[Answer]: A

### Question 4: Persistence and transaction ownership

Which dependency pattern should isolate SQLite access and atomic cooking/undo operations? Neither option permits UI/model direct database writes; final transaction algorithms remain for Functional Design.

A) Domain-specific repository interfaces with a transaction boundary coordinated by the cooking application service. Recipe, inventory, plan and event persistence remain explicit interfaces; one commit groups stock, event, version and idempotency changes, with rollback on failure.

B) A cohesive persistence gateway owns repository access and exposes explicit atomic confirm/undo operations; cooking services call those operations, while the gateway controls stock/event/version/idempotency consistency. The gateway exposes typed domain operations, not arbitrary SQL execution.

X) Other (specify persistence access, transaction ownership and atomicity boundaries after [Answer]:).

[Answer]: B

### Question 5: Long-running operation communication

How should Streamlit/FastAPI communicate for bounded model or recipe retrieval requests? This decides request/result presentation, not new background functionality or concurrency support.

A) One request returns a final typed result or clear error; show a waiting state, enforce existing timeouts, and expose diagnostic tool traces for later evaluation. No token/progress streaming or background jobs.

B) Stream progress/tool-status events during the same bounded request and return a final typed result/error. Progress is informational, never confirmation or write authorization; interrupted requests must not imply successful saves or deductions. No background jobs or additional concurrent users.

X) Other (describe bounded communication and interruption semantics after [Answer]:).

[Answer]: A

## 5. Why these questions are needed

| Rule category                  | Question coverage | Already fixed and not re-asked                              |
| ------------------------------ | ----------------- | ----------------------------------------------------------- |
| Component identification       | Q1                | User scope and supported capabilities                       |
| Component methods/contracts    | Q2                | Deterministic validation and supported units                |
| Service layer/orchestration    | Q3                | Explicit tool loop, no model-authorized writes              |
| Dependencies and communication | Q4/Q5             | Localhost, SQLite, FastAPI/Streamlit, no concurrent editing |
| Design patterns                | Q1/Q3/Q4          | No mandatory orchestration framework, cloud or vector DB    |

The validated selection is Q1 A / Q2 A / Q3 A / Q4 B / Q5 A. Capability-centered organization is compatible with a shared persistence gateway when each capability retains explicit responsibilities and the gateway owns atomic writes. Typed errors apply consistently across planning, cooking and import services. Answers selected the directions; the subsequent explicit team message approved this plan.

## 6. Finalized design directions — plan approved

- **Component organization (Q1 A):** Capability-centered logical modules for Preferences, Recipes with retrieval/import adapters, Inventory, Planning/Shopping, and Cooking Events, with shared deterministic quantity/constraint policies. Streamlit calls FastAPI; neither UI nor model accesses SQLite or model keys directly. This is logical organization, not a microservice or work-unit decision.
- **Error handling (Q2 A):** Successful methods return typed results. Document typed validation, not-found, unsupported-source, infeasibility, stale/insufficient stock, conflicting-request and provider-unavailable errors; map them centrally at API/tool boundaries. Replaying a successful confirmation returns its recorded result without a second effect, not a new failure solely because it is a replay. Conflicting reuse of an identifier remains an explicit error. No ambiguous success strings or silently ignored errors.
- **Agent coordination (Q3 A):** A dedicated planning orchestration service owns the bounded model/tool loop for planning and remaining-meal revision, using inventory, recipe and deterministic validation interfaces. Separate cooking/undo and import services handle those application workflows. The model proposes; deterministic backend checks enforce user confirmation and persistent-write boundaries. A successfully validated proposal is not permission to save it.
- **Database transactions (Q4 B):** A cohesive persistence gateway owns SQLite repository access and exposes typed domain operations, including atomic confirm/undo. Cooking services invoke these operations, but the gateway controls stock/event/version/idempotency consistency and rollback. Capability boundaries remain visible; the gateway does not become an arbitrary SQL interface or a source of model instructions. Detailed transaction algorithms/schemas are deferred to Functional Design.
- **Request presentation (Q5 A):** Bounded request/final-result interaction, with a waiting state and a final typed result or clear error. Retain diagnostic tool traces for later evaluation. No token/progress streaming or background jobs; preserve the 60-second model timeout and explicit confirmation boundaries. A transport interruption is not evidence that a write succeeded or failed; confirmation identifiers and recorded outcomes support safe result recovery without repeating effects.

### Compatibility review

All five answer fields are populated with valid choices, without combined or conditional answers. They preserve the approved local stack, single-profile scope, explicit tool loop, source eligibility, deterministic quantity/exclusion checks, no negative stock, idempotent confirmation and safe undo. Planning orchestration and the persistence gateway have distinct responsibilities: the former coordinates proposals, while write-capable application services enforce confirmation and the gateway guarantees atomic persistence. No new feature, source provider, endpoint implementation, dependency version or deployment topology is selected here. No follow-up question is necessary.

## 7. Extension applicability and stop conditions

Security and Resiliency extensions are disabled; the approved explicit safety/integrity requirements remain binding. PBT-01 is N/A for its Functional Design-specific property inventory; PBT-02–PBT-08 and PBT-10 implementation checks are N/A without code/test generation; PBT-09 configuration verification is N/A without a Construction NFR/dependency stage. Retain all full-PBT obligations and Hypothesis/pytest direction in design traceability; no testing compliance is claimed.

After answers are validated, present the finalized plan for explicit approval. Only after that approval generate the five English design artifacts. After their separate approval, proceed to Units Generation planning. Stop after approved Units Generation; do not enter Construction. No application code, live provider calls, external-service access or data acquisition occurs during this design stage.

## Inception closeout
As of 2026-11-20T19:58:21Z, all required Inception stage artifacts are explicitly approved, including final unit artifacts. This plan's tasks are complete; Construction has not started.
