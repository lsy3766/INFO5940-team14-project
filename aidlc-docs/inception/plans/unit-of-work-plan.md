# Unit of Work Generation Plan

Status: Plan explicitly approved by the team, recorded at 2026-11-20T19:55:45Z. Three unit artifacts generated under A/A/A/A choices and explicitly approved. Units Generation and Inception complete; STOP.

## 1. Baseline and terminology

Inputs: approved requirements.md; stories.md (US-01–US-19 with 52 scenarios); personas.md; execution-plan.md; and components.md, component-methods.md, services.md, component-dependency.md and application-design.md under inception/application-design/.

A **Unit of Work** is the planning grouping of stories for development. Under the AI-DLC monolith convention, one application-level unit contains internal logical modules; independently deployable services would be separate units only with an actual approved deployment need. A **Module** is an internal responsibility boundary, not an independently deployed service. Application Design's “application services” are logical orchestration roles and do not contradict this terminology. Streamlit and FastAPI runtime entry points do not, by themselves, require separate work units or independently released services.

The approved local application baseline has no independent deployment/scaling requirements. Plan one whole-application work unit with capability-centered modules rather than inventing five services for five teammates. This follows the stage's monolith convention; no new deployment architecture is introduced. Module grouping and collaboration choices are recorded below; the plan is explicitly approved; artifact approval explicitly received. If the team identifies a genuine need to alter approved deployment boundaries, document and review the architecture impact before decomposition rather than silently adding services.

Keep all approved constraints, C01–C10 responsibilities, A/A/A/B/A design decisions and gateway-owned atomicity. In particular, no split may scatter stock/event/version/idempotency commits across independently owned stores. The model remains read/compute-only; UI uses FastAPI, not SQLite/provider keys. No new accounts, live search, portion inventory or paid dependencies.

## 2. Required outputs and proposed content

- inception/application-design/unit-of-work.md: unit identity, scope/inclusions/exclusions, responsibility boundaries, internal module catalog tied to C01–C10, interfaces/shared resources, Greenfield code organization strategy and future verification obligations. Paths are proposals/documentation only; no source directories or application files are created.
- inception/application-design/unit-of-work-dependency.md: one-unit external/dependency view, internal module dependency matrix, authoritative-state ownership, integration/contract checkpoints and future sequencing constraints. Clearly label internal module edges as not inter-service communication.
- inception/application-design/unit-of-work-story-map.md: all 19 stories assigned to the whole-application unit with one primary internal module and explicit collaborators; carry all FR/NFR/AC and Q-01–Q-06 quality obligations without orphaned recovery stories or duplicated ownership claims.

Exact module names and story assignments are generated only after choices and plan approval. No parallel agent work, code ownership assignment, implementation or build/testing activity is authorized by this plan.

## 3. Execution checklist

### Part 1 — Planning
- [x] Read Units Generation rules and approved application context.
- [x] Record explicit approval of all five design artifacts; close Application Design tracking.
- [x] Distinguish work units, internal modules and logical orchestration roles using the approved local baseline.
- [x] Assess all decomposition question categories and prepare this plan with mandatory artifacts.
- [x] Inspect the reported submission: all four answer fields in the received file are blank.
- [x] Collect all four actual answers without preselection.
- [x] Analyze completeness, consistency, boundary clarity and compatibility with approved design.
- [x] Check ambiguity: none remains; no follow-up questions required.
- [x] Incorporate confirmed answers and log the complete plan approval prompt.
- [x] Receive explicit unit-plan approval before generation.

### Part 2 — Generation only after plan approval
- [x] Re-read approved unit plan/context and identify first incomplete generation step.
- [x] Generate unit-of-work.md with whole-application unit boundaries, module responsibilities and Greenfield code organization strategy.
- [x] Generate unit-of-work-dependency.md with dependency matrix, shared-contract integration and transaction ownership boundaries.
- [x] Generate unit-of-work-story-map.md assigning every US-01–US-19 and retaining complete requirement/quality traceability.
- [x] Validate no missing or multiply accountable stories, no circular module dependencies, no split atomic stock ownership and no artificial independently deployed services.
- [x] Validate all diagram syntax/Markdown before writing, include text alternatives, evaluate enabled extensions and update task/state checkboxes immediately.
- [x] Log/present unit artifact approval request.
- [x] Receive explicit team unit-artifact approval.
- [x] After explicit approval, mark Units Generation and Inception complete and STOP. Do not enter Construction.

### Separate approvals
- [x] Team explicitly approves unit-of-work-plan.md.
- [x] Team explicitly approves all three generated unit artifacts.

## 4. Team questions

Fill every [Answer]: with a letter; use X for a specific alternative compatible with approved scope. Submitting answers does not approve the plan.

### Question 1: Internal story grouping and business boundaries

Within the one whole-application unit, how should approved capabilities map to internal module ownership? Both alternatives preserve C01–C10 contracts and keep the transaction gateway cohesive.

A) Keep Preferences, Recipes, Inventory, Planning/Shopping, and Cooking Events as primary business module boundaries. Keep UI/API, deterministic policies, planning orchestration, external adapters and persistence gateway as explicit supporting modules. Stories map to one primary business module plus supporting collaborators.

B) Use three broader internal business groupings: Profile/Stock (with separate Preferences and Inventory responsibilities), Recipe Access, and Meal Lifecycle (with distinct Planning/Shopping and Cooking Events submodules). Preserve the same supporting boundaries and one primary submodule per story; broader groups are review/navigation containers, not merged write authority.

X) Other (specify grouping and single-primary-owner rules without changing approved component responsibilities after [Answer]:).

[Answer]: A

### Question 2: Dependency and shared-contract integration

How should the unit documentation describe shared contracts and future integration? No integration tests or code are created now.

A) Maintain one shared typed contract catalog with capability owners reviewing changes; establish documented integration checkpoints around recipe eligibility, planning validation and atomic cooking/undo. Use the approved dependency matrix to define allowed calls, and flag contract changes for affected reviewers before later implementation.

B) Document typed contracts alongside each capability and maintain a consolidated compatibility index; contract consumers review changes, with the same recipe/planning/cooking checkpoints and approved dependency restrictions. Do not maintain conflicting copies of the same type definition.

X) Other (describe authoritative contract ownership, consumer compatibility and integration checkpoints after [Answer]:).

[Answer]: A

### Question 3: Team alignment without deployment-driven assignments

How should module coordination and review responsibilities be recorded? Existing five role assignments remain unchanged and do not imply independent services or authority to approve for absent teammates.

A) Use shared module ownership, with the existing role leads coordinating review in their areas. Do not assign exclusive implementation ownership now; any future coding allocation is a separate team decision.

B) Record primary and secondary module coordinators now, while retaining shared review and one application unit. Provide the actual agreed module-to-member assignments after [Answer]:; do not derive them from member order or existing role names alone.

X) Other (describe collaboration and review boundaries after [Answer]:).

[Answer]: A

### Question 4: Greenfield code organization documentation

Which proposed source layout should be documented for later Construction? This is documentation only. UI/API remain separate entry points, and all production writes use the gateway. Application code never belongs under aidlc-docs/.

A) One root app/ package with capability directories (preferences, recipes, inventory, planning, cooking) and supporting directories (policies, orchestration, adapters, persistence); app/ui.py and app/api.py remain planned entry points; tests/ mirrors capabilities with a separate integration area. Shared types have one authoritative location consistent with Q2.

B) One root src/cooking_assistant/ package with the same capability/supporting divisions, separate ui and api entry modules, and root tests/. Record the change from the technical input's illustrative app.* entry-point paths explicitly; no package/install setup is generated now. Shared types remain authoritative as selected in Q2.

X) Other (give a concrete single-application source/test organization respecting approved boundaries after [Answer]:).

[Answer]: A

## 5. Category coverage and non-applicability

| Rule category | Treatment and evidence |
| --- | --- |
| Story grouping | Q1 and complete story map output; primary module and collaborators, including recovery stories. |
| Dependencies | Q2, approved C01–C10 dependency matrix and atomic gateway ownership; inter-unit communication N/A under one application unit. |
| Team alignment | Q3; preserve five reported responsibilities without inferring new assignments or services. |
| Technical considerations | Already settled local stack, single profile/no concurrency, no independent scaling or deployment needs; not re-asked. Runtime entry points are documented, not separate units. |
| Business domain | Q1 preserves recipe eligibility, inventory and cooking state boundaries; shared policies/gateway do not absorb domain authority. |
| Code organization | Greenfield multi-unit-specific rule N/A because one unit; Greenfield organization documentation is still mandatory and Q4 collects a concrete layout preference. |

No runtime feasibility tests, external-source investigation, model access or dependency installation are needed or performed to draft this plan. Exact source providers, schemas, rounding/undo predicates and implementation caps remain deferred design/Construction checkpoints already recorded in application-design.md, not silently resolved here.

## 6. Finalized selections — plan explicitly approved

Answers received in chat: Q1 A, Q2 A, Q3 A, Q4 A. The punctuation in “4,A” is interpreted as the clearly stated Q4 A, not an additional choice. All four choices are complete and mutually consistent with approved capability-centered design and cohesive persistence gateway ownership.

- Module boundaries: one whole-application work unit contains Preferences, Recipes, Inventory, Planning/Shopping and Cooking Events business modules, plus explicit UI/API, deterministic policies, planning orchestration, external adapters and persistence supporting modules. Map every story to one primary business module and named supporting collaborators; modules are not independently deployed services.
- Interfaces: one authoritative shared typed contract catalog; capability owners review changes and affected consumers review compatibility. Document recipe eligibility, planning validation and atomic cooking/undo integration checkpoints. Follow the approved allowed-call matrix; no interface can bypass backend confirmation or split transaction ownership.
- Collaboration: shared module ownership with existing role leads coordinating review in their areas. No exclusive coding assignments or fabricated module coordinators; future implementation allocation requires a separate team decision. Five people do not imply five services.
- Code organization: document a future root app/ package with preferences/, recipes/, inventory/, planning/, cooking/ and supporting policies/, orchestration/, adapters/, persistence/; retain planned app/ui.py and app/api.py. Document one shared contracts location consistent with the authoritative catalog and tests/ with capability and integration areas. Exact layout is recorded in unit-of-work.md after plan approval; no folders or code are created now.

Compatibility: Q1 A directly preserves C01–C10 capability responsibilities; Q2 A preserves typed success/error contracts; Q3 A does not override existing role assignments; Q4 A retains the technical input's planned entry points. No scope, deployment, model access or transaction behavior changes. Answer submission is not approval.

## 7. Extension compliance and final stop

Security and Resiliency extensions remain disabled; approved explicit security/integrity boundaries remain binding. PBT-01 formal identification N/A until Functional Design; PBT-02–PBT-08/PBT-10 implementation verification N/A without test/code generation; PBT-09 configuration N/A without future NFR/dependency work. Carry all full-PBT and example-test obligations into unit responsibility/quality mappings, with Hypothesis/pytest direction retained. No applicable blocking extension findings at this planning stage.

After answers are validated, request explicit unit-plan approval before producing the three artifacts. Their later approval completes this Inception run and triggers STOP, overriding generic next-stage wording that would otherwise suggest Construction. No application code, tests, source directories, deployments or external connections are created in this stage.
