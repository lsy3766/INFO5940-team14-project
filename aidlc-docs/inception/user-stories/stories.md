# Cooking Assistant — User Stories

Status: Explicitly approved by the team, recorded at 2026-11-20T19:27:57Z. Approval covers this artifact; execution plan and subsequent artifacts require separate approval. No implementation, test execution, live access, or measured success is claimed.

Baseline: ../requirements/requirements.md. Organization: feature-based groups, with a separate journey and complete requirement coverage below. All concrete data are illustrative acceptance fixtures, not real recipes or interview evidence. P-01 and P-02 are defined in personas.md. Story prerequisites are behavior/data dependencies, not sprint ordering.

## US-01 — Set preferences and meal constraints

Feature: Preferences. Personas: P-01, P-02.

As a local cook, I want to record preferences and serving/time overrides so recommendations fit my needs.

Requirements: FR-01, FR-08. Dependencies / preconditions: None. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-01-AC1** — **Given** a new local profile, **When** I set exclusions and save party size 1, **Then** the values persist and default breakfast/lunch/dinner budgets are 15/45/45 minutes.
- **US-01-AC2** — **Given** the profile is editable, **When** I save party size 10 and a meal override of 2 diners and 20 minutes, **Then** both valid counts persist and the override applies only to that meal.
- **US-01-AC3** — **Given** existing valid preferences, **When** I submit 0, 11, a non-integer serving count, or invalid time input, **Then** validation rejects the invalid input without replacing the saved values.

## US-02 — Maintain inventory

Feature: Inventory. Personas: P-01, P-02.

As a local cook, I want to correct my stock so later calculations use current quantities.

Requirements: FR-06. Dependencies / preconditions: None. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-02-AC1** — **Given** rice stock is 500 g, **When** I manually add 250 g using Save, **Then** stock becomes 750 g and that direct Save is confirmation of this manual edit.
- **US-02-AC2** — **Given** rice stock is 750 g, **When** I save a correction to 600 g, then remove the entry using the explicit manual removal action, **Then** the recorded values reflect each confirmed edit; no cooking deduction is invented.
- **US-02-AC3** — **Given** a stock-edit form, **When** I cancel an edit or submit a negative or non-finite quantity, **Then** cancellation leaves stock unchanged and invalid quantities are rejected.

## US-03 — Find attributed usable recipes

Feature: Recipe access. Personas: P-01, P-02.

As a local cook, I want to find suitable recipes so I can plan from traceable information.

Requirements: FR-02, FR-03, FR-04, FR-05. Dependencies / preconditions: None; eligible corpus is a data prerequisite. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-03-AC1** — **Given** an acceptance corpus, **When** eligible complete distinct records are counted, **Then** there are at least 45, covering Chinese and Western recipes and all three meal types; each has identity, title, ingredients/quantities/units, base servings, instructions, meal tags, preparation time, provenance and retrieval date.
- **US-03-AC2** — **Given** eligible candidates plus incomplete/unreviewed records, **When** I search by keyword, ingredients and meal tags, **Then** matching eligible candidates are retrievable locally with source information; incomplete/unreviewed records do not enter automatic plans or deductions.
- **US-03-AC3** — **Given** fewer than 45 eligible records or insufficient suitable candidates, **When** coverage is assessed or a request is evaluated, **Then** the shortfall is reported rather than filled with fabricated recipes or sources; expansion above 45 is not a separate acceptance blocker.

## US-04 — Import an existing public recipe

Feature: Recipe access. Personas: P-01, P-02.

As a local cook, I want to import an existing recipe so I can reuse a permitted source.

Requirements: FR-02, FR-04, FR-05. Dependencies / preconditions: US-03 record-eligibility rules. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-04-AC1** — **Given** a supported public URL with permitted use and complete Recipe metadata, **When** I request import and confirm the previewed persistent change, **Then** a validated attributed reusable record is saved, including retrieval date; fetching alone does not silently persist it.
- **US-04-AC2** — **Given** an incomplete, unsupported, or blocked page, **When** I attempt import, **Then** a clear error or missing-field explanation is shown; the record is not eligible for automatic planning or deduction and no values or permissions are invented.
- **US-04-AC3** — **Given** an import preview, **When** I cancel, **Then** no persistent change or stock deduction occurs.

## US-05 — Recover a failed import with source text

Feature: Recipe access recovery. Personas: P-01, P-02.

As a local cook, I want to provide missing source information so a failed import can become usable.

Requirements: FR-04, FR-05, FR-16. Dependencies / preconditions: US-04 failed import or missing data. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-05-AC1** — **Given** a failed import, **When** I paste source text and complete missing quantities/base servings, **Then** the content is labelled as user-provided, checked for permitted use/provenance and completeness, and shown for confirmation before saving.
- **US-05-AC2** — **Given** source eligibility or required data remains unresolved, **When** I attempt to use it automatically, **Then** the record remains ineligible and the unresolved issue is explained; original recipe authoring is not required.

## US-06 — Generate a five-day plan

Feature: Meal planning. Personas: P-01, P-02.

As a local cook, I want to receive a useful meal proposal so I can decide what to eat.

Requirements: FR-07, FR-08, FR-15. Dependencies / preconditions: US-01, US-03; inventory snapshot available via US-02. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-06-AC1** — **Given** valid preferences, eligible suitable recipes and a stock snapshot, **When** I request a plan, **Then** the agent reads preferences/inventory, retrieves source recipes, obtains deterministic validation, and returns five days with breakfast/lunch/dinner: 15 slots.
- **US-06-AC2** — **Given** default constraints and no permission to relax them, **When** a plan is proposed, **Then** exclusions are honored, breakfasts fit 15 minutes and lunches/dinners 45 minutes, and no identical main dish occurs on consecutive days; quantities and any shortages are visible.
- **US-06-AC3** — **Given** a proposed plan, **When** I have not confirmed saving, **Then** neither saved plan nor stock changes; the system does not claim tool success without successful results.

## US-07 — Relax a feasible soft constraint explicitly

Feature: Meal planning recovery. Personas: P-01, P-02.

As a local cook, I want to choose an allowed constraint change so I can recover an infeasible request.

Requirements: FR-08, FR-15, FR-16. Dependencies / preconditions: US-06 infeasibility or limited candidates. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-07-AC1** — **Given** no candidate meets a 15-minute breakfast budget but an exclusion-safe 20-minute option exists, **When** I approve the proposed time relaxation, **Then** a revised proposal uses that permission and deterministic validation; no exclusion is relaxed.
- **US-07-AC2** — **Given** the same proposal, **When** I decline relaxation, **Then** no unauthorized relaxed plan is saved and a correct limitation is reported.
- **US-07-AC3** — **Given** the only candidates violate exclusions, **When** I request a solution, **Then** the system reports infeasibility instead of silently overriding exclusions or inventing eligible recipes.

## US-08 — Adjust shared dishes and scale servings

Feature: Meal planning. Personas: P-01, P-02.

As a local cook, I want to scale shared dishes so quantities match the diners.

Requirements: FR-01, FR-07, FR-10. Dependencies / preconditions: US-01, US-03; US-06 when editing a plan. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-08-AC1** — **Given** a recipe requires 150 g rice for 2 servings, **When** I preview it for 4 diners, **Then** the required rice is 300 g; combinations of dishes are allowed without a one-dish-per-person assumption.
- **US-08-AC2** — **Given** a meal serving override, **When** I change it within 1–10, **Then** only the affected proposed meal quantities change; stock remains unchanged.
- **US-08-AC3** — **Given** an ingredient expressed in count, **When** a calculation would require count-to-weight or density conversion, **Then** the system flags incompatibility rather than inventing a conversion; substitutions are not silently applied.

## US-09 — View an inventory-aware shopping list

Feature: Shopping. Personas: P-01, P-02.

As a local cook, I want to see shortages so I can shop without double-counting stock.

Requirements: FR-10. Dependencies / preconditions: US-02; US-06 or selected eligible dishes. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-09-AC1** — **Given** two selected dishes need 300 g and 400 g rice and stock is 0.5 kg, **When** I generate the list, **Then** total need is 700 g, stock credit is 500 g once, and shortage is 200 g; stock is unchanged.
- **US-09-AC2** — **Given** stock exceeds a compatible-unit ingredient requirement, **When** I generate the list, **Then** the shortage is zero, not negative.
- **US-09-AC3** — **Given** requirements combine incompatible units such as g and ml without a reviewed conversion, **When** I generate the list, **Then** incompatibility is reported instead of silently combining them.

## US-10 — Replace, skip and save meal changes

Feature: Meal changes. Personas: P-01, P-02.

As a local cook, I want to adapt suggested meals so the plan remains useful.

Requirements: FR-09, FR-11. Dependencies / preconditions: US-06; US-08 for serving changes. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-10-AC1** — **Given** a saved plan, **When** I replace or skip a meal in a draft, **Then** the affected draft slot changes but saved plan and stock remain unchanged until the relevant save/confirmation.
- **US-10-AC2** — **Given** a valid agent-proposed revision, **When** I preview and explicitly confirm it, **Then** the revised plan persists without stock deduction; cancellation preserves the saved plan.
- **US-10-AC3** — **Given** a direct manual plan edit, **When** I explicitly save it, **Then** that Save confirms the manual edit; it does not trigger inventory deduction or an unrequested cooking event.

## US-11 — Preview actual cooking quantities

Feature: Cooking. Personas: P-01, P-02.

As a local cook, I want to review actual use so stock reflects what I cooked.

Requirements: FR-11. Dependencies / preconditions: US-02, US-03; saved plan is optional. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-11-AC1** — **Given** a library or planned recipe estimates 300 g rice and stock is 500 g, **When** I select it and correct actual use to 250 g, **Then** the preview shows 250 g and prospective remaining stock 250 g; current stock stays 500 g.
- **US-11-AC2** — **Given** a valid consumption preview, **When** I cancel or only view it, **Then** there is no deduction or committed cooking event.
- **US-11-AC3** — **Given** an invalid or incompatible actual quantity, **When** I request a preview, **Then** validation explains the problem; no stock changes.

## US-12 — Confirm cooking exactly once

Feature: Cooking. Personas: P-01, P-02.

As a local cook, I want to confirm my cooking so stock is deducted reliably.

Requirements: FR-12, FR-15. Dependencies / preconditions: US-11 fresh sufficient-stock preview. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-12-AC1** — **Given** a fresh valid preview using 250 g from 500 g stock, **When** I explicitly confirm with one request identifier, **Then** stock becomes 250 g and one cooking event records the deduction atomically.
- **US-12-AC2** — **Given** that committed confirmation, **When** the same confirmation is submitted again, **Then** the same recorded outcome has no second stock deduction or duplicate cooking effect.
- **US-12-AC3** — **Given** a preview not yet confirmed, **When** the model proposes committing it without user confirmation, **Then** backend logic refuses the write; partial stock changes do not occur on failure.

## US-13 — Recover a stale cooking preview

Feature: Cooking recovery. Personas: P-01, P-02.

As a local cook, I want to refresh stale information so I can confirm against current stock.

Requirements: FR-12. Dependencies / preconditions: US-11 and subsequent US-02 stock edit. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-13-AC1** — **Given** a preview made at stock version 7, **When** stock is changed before I confirm, **Then** confirmation is rejected as stale and no deduction is made.
- **US-13-AC2** — **Given** the rejected preview, **When** I request a fresh preview using current stock and confirm it if valid, **Then** the revised quantities/state are visible before confirmation; only the fresh valid confirmation can commit.

## US-14 — Resolve insufficient cooking stock

Feature: Cooking recovery. Personas: P-01, P-02.

As a local cook, I want to correct quantities or stock so I can record valid consumption.

Requirements: FR-11, FR-12. Dependencies / preconditions: US-11 shortage preview. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-14-AC1** — **Given** stock is 100 g and proposed use is 150 g, **When** I attempt confirmation, **Then** it is rejected with a 50 g shortage; stock stays 100 g.
- **US-14-AC2** — **Given** that shortage, **When** I correct stock by an explicit manual edit or correct actual use and request a fresh preview, **Then** confirmation remains blocked until the new preview is fresh and sufficient; a valid confirmed 80 g use from 100 g leaves 20 g, never a negative balance.

## US-15 — Undo a cooking deduction safely

Feature: Cooking recovery. Personas: P-01, P-02.

As a local cook, I want to reverse an erroneous deduction so my stock can be corrected without erasing history.

Requirements: FR-13. Dependencies / preconditions: US-12 committed event. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-15-AC1** — **Given** 500 g stock was reduced to 250 g by one event and no conflicting changes exist, **When** I explicitly confirm its undo, **Then** a compensating event restores 250 g, leaving 500 g, while preserving the original event.
- **US-15-AC2** — **Given** that event was already undone, **When** I retry undo, **Then** no second restoration occurs.
- **US-15-AC3** — **Given** a later change makes the requested undo unsafe under current-state validation, **When** I request undo, **Then** the operation is rejected with a clear reason and no partial change; arbitrary history deletion is not offered.

## US-16 — Review remaining-meal adjustments

Feature: Meal changes. Personas: P-01, P-02.

As a local cook, I want to review stock-aware revisions so future meals remain realistic.

Requirements: FR-13, FR-15. Dependencies / preconditions: US-06 and a confirmed stock change such as US-12 or US-15. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-16-AC1** — **Given** confirmed cooking changes stock and remaining meals need unavailable ingredients, **When** the agent checks remaining meals, **Then** it shows shortages and proposes validated changes, leaving the saved plan unchanged.
- **US-16-AC2** — **Given** a proposed adjustment, **When** I explicitly accept it or cancel, **Then** acceptance saves the validated revision without new cooking deductions; cancellation preserves the plan.

## US-17 — Record takeout and leftovers

Feature: Meal annotations. Personas: P-01, P-02.

As a local cook, I want to mark what I ate so the plan reflects reality without duplicate raw-ingredient use.

Requirements: FR-14. Dependencies / preconditions: US-10 annotation/save behavior; original event for leftover context. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-17-AC1** — **Given** a meal slot, **When** I explicitly save a takeout annotation, **Then** the annotation persists and raw-ingredient stock is unchanged.
- **US-17-AC2** — **Given** original cooking already deducted 250 g rice, **When** I annotate a later meal as leftovers from that cooking, **Then** no second raw-ingredient deduction or leftover-portion stock is created.
- **US-17-AC3** — **Given** I cooked more than I ate immediately, **When** I record actual cooking consumption via US-11/US-12, **Then** deductions reflect cooking use, not the fraction immediately eaten; later leftover annotation does not repeat them.

## US-18 — Approve a substitution variant

Feature: Recipe access. Personas: P-01, P-02.

As a local cook, I want to approve a changed recipe so substitutions do not silently alter quantities.

Requirements: FR-10, FR-04. Dependencies / preconditions: US-03 complete eligible recipe. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-18-AC1** — **Given** a proposed ingredient substitution, **When** I have not approved the complete variant, **Then** the original recipe remains the basis for planning and stock calculations.
- **US-18-AC2** — **Given** a variant has complete quantities, compatible units and reviewable provenance, **When** I explicitly approve saving it, **Then** it may be used only after eligibility and exclusion validation; unsupported conversions remain blocked.

## US-19 — Recover from model or tool failure

Feature: Service recovery. Personas: P-01, P-02.

As a local cook, I want to receive an honest failure and retry option so I retain control.

Requirements: FR-15, FR-16. Dependencies / preconditions: A user-initiated operation from US-04, US-06 or US-16. Cross-cutting obligations Q-01–Q-06 apply where relevant.

- **US-19-AC1** — **Given** a model quota/access error or failed retrieval/validation tool, **When** the operation fails, **Then** a clear limitation and recovery action appear, with no fabricated success, unintended write, paid fallback or enabled billing.
- **US-19-AC2** — **Given** a model request has not completed within 60 seconds, **When** the timeout is reached, **Then** the request ends with a timeout explanation and user-controlled retry; no indefinite retry occurs.
- **US-19-AC3** — **Given** a prior failed request, **When** I choose retry after the underlying issue is resolved, **Then** the operation obtains current data and validations again; prior confirmation is not reused to authorize unrelated writes.

## Separate end-to-end journey

1. Set one local profile's exclusions, diner count and meal budgets (US-01); correct inventory explicitly (US-02).
2. Retrieve eligible attributed recipes (US-03), optionally importing a supported source and recovering missing information without inventing it (US-04/05).
3. Obtain a 15-slot proposal with validated constraints (US-06), approve any time/repetition relaxation if needed (US-07), and scale shared dishes (US-08).
4. Review aggregated shortages (US-09); replace/skip meals and explicitly save the chosen plan (US-10). None of these steps deducts stock.
5. Select what was actually cooked, correct consumption, preview (US-11), and confirm once (US-12). If stock changed or is insufficient, refresh or correct before a new confirmation (US-13/14).
6. Undo an erroneous event safely if needed (US-15); review proposed remaining-meal changes (US-16); annotate takeout/leftovers without repeat deductions (US-17).
7. Approve substitution variants only through reviewed eligible records (US-18); recover quota/retrieval failures explicitly (US-19).

These are user interactions, not required implementation sequence. A user may start with an eligible library recipe without first saving a plan.

## Dependency and recovery summary

| Normal capability | Distinct-action recovery | Shared constraint |
| --- | --- | --- |
| US-04 import | US-05 paste/correct source data | Eligibility before automatic use |
| US-06 plan | US-07 approve permitted relaxation | Exclusions stay hard |
| US-11/12 preview/confirm | US-13 refresh stale data; US-14 correct shortage | Fresh validated preview; zero negative stock |
| US-12 committed event | US-15 confirm safe undo | Once-only compensation and preserved history |
| US-06/16 model/tool operation | US-19 user-controlled retry | No fabricated success or paid fallback |

US-03 data eligibility is reused rather than duplicated in import and variant stories. US-10 save/confirmation rules apply to plan/annotation changes. Dependencies are explicitly acknowledged; stories claim independently observable value, not absence of shared prerequisites.

## Cross-cutting acceptance and quality obligations

- **Q-01 — Local operation and stack:** Future delivery uses Python 3.12.x, venv/pip, FastAPI/Pydantic v2, Streamlit, SQLite/sqlite3, HTTPX, Beautiful Soup 4, pytest/TestClient and a small explicit provider-SDK tool loop. Bind localhost, keep persistent app data local, pin compatible releases at Construction start. No cloud hosting, orchestration framework or vector database is required. Recipe access after collection is local; hosted generation is not promised offline.
- **Q-02 — Free-model feasibility and integrity:** Planned Gemini Developer API gemini-3.5-flash-lite remains subject to account, quota, tool-calling and availability checks in Construction. Stop/report failures; no silent paid switch, billing enablement or assumed Ollama fallback. Model knowledge is not a verified recipe database. Source terms, supported sites and corpus availability must be checked, not claimed already verified.
- **Q-03 — Input and fetch safety:** Validate finite quantities, serving bounds, compatible units and exclusions server-side. Keys stay server-side in ignored configuration and never in logs/commits; use fictional demo profiles. Web/pasted text is untrusted data, not tool instructions. Accept public HTTP(S) only; block private/loopback destinations and unsafe redirects; bound download size/time. Use validated JSON and allowlisted functions, not unsafe pickle/YAML or eval/exec of generated content. These are inherited requirements, not a newly enabled security extension.
- **Q-04 — Usability and timing:** English core flows must be keyboard-operable, with editable previews, clear source labels, shortages and errors. Non-model local actions target completion within 2 seconds; model requests time out within 60 seconds with a clear retry action. Do not guarantee allergy safety, infer spoilage or give medical nutrition advice. Performance remains a future measurement, not a result of story review.
- **Q-05 — Agent evaluation:** On 10 fixed scenarios, at least 8 yield a usable valid plan or correct infeasibility explanation. All 10 preserve hard exclusions and write-confirmation boundaries. Inspect actual traces for recipe retrieval, inventory access, deterministic validation and revision when needed. A feasible plan can be saved with at most one requested revision. Fixtures must include successful planning and failure/recovery cases; correct infeasibility is not fabricated success.
- **Q-06 — Test strategy and state integrity:** Use deterministic fake model/tool responses for most tests; live agent evaluation occurs only in Construction. Critical paths require explicit example-based tests plus full applicable Hypothesis/pytest PBT. Identify properties, test serialization round trips, arithmetic/stock invariants, idempotency, oracle comparison where applicable, and stateful operation sequences against a reference model. Use reusable domain generators including boundaries, shrinking, reproducible seeds and CI execution. Preserve atomic transactions, stock-version checks, idempotency and safe compensation; test both result and lack of unintended mutation. No tests, CI or dependencies are created by these documents.

## Complete requirement coverage matrix

Every approved identifier is mapped below; mappings specify scenario IDs or quality obligations rather than claiming implemented compliance.

| Approved requirement | Story criteria / quality coverage |
| --- | --- |
| FR-01 | US-01-AC1–3; US-08-AC1–2 |
| FR-02 | US-03-AC1–2; US-04-AC1 |
| FR-03 | US-03-AC1/3; Q-02 |
| FR-04 | US-03-AC1–3; US-04-AC2; US-05-AC1–2; US-18-AC2 |
| FR-05 | US-03-AC2; US-04-AC1–2; US-05-AC1–2 |
| FR-06 | US-02-AC1–3 |
| FR-07 | US-06-AC1–3; US-08-AC1–2 |
| FR-08 | US-01-AC1; US-06-AC2; US-07-AC1–3 |
| FR-09 | US-10-AC1–3 |
| FR-10 | US-08-AC1–3; US-09-AC1–3; US-18-AC1–2 |
| FR-11 | US-11-AC1–3; US-10-AC2; US-14-AC2 |
| FR-12 | US-12-AC1–3; US-13-AC1–2; US-14-AC1–2 |
| FR-13 | US-15-AC1–3; US-16-AC1–2 |
| FR-14 | US-17-AC1–3 |
| FR-15 | US-06-AC1/3; US-07-AC1; US-12-AC3; US-16-AC1–2; US-19-AC1; Q-05 |
| FR-16 | US-05-AC2; US-07-AC3; US-19-AC1–3; Q-02 |
| NFR-01 | Q-01 |
| NFR-02 | Q-01 |
| NFR-03 | Q-02; US-19-AC1 |
| NFR-04 | Q-04; US-19-AC2 |
| NFR-05 | Q-03; US-01-AC3; US-02-AC3; US-08-AC3 |
| NFR-06 | Q-03; US-04-AC2 |
| NFR-07 | Q-06; US-12–US-15 |
| NFR-08 | Q-06 |
| NFR-09 | Q-04; US-03-AC2; US-11-AC1–3; US-19-AC1–2 |
| AC-01 | US-01-AC1–3; US-06-AC1–3; US-07-AC1–3; US-08-AC1–2 |
| AC-02 | US-03-AC1–3; US-04-AC1–2; US-05-AC1–2 |
| AC-03 | US-08-AC1–3; US-09-AC1–3; Q-06 |
| AC-04 | US-02-AC1–3; US-06-AC3; US-10-AC1–3; US-11-AC1–3 |
| AC-05 | US-12–US-17; Q-06 |
| AC-06 | US-04-AC2; US-19-AC1–3; Q-01–Q-04 |
| AC-07 | US-06-AC1/3; US-16-AC1; Q-05 |
| AC-08 | Q-06 |

## INVEST review and residual dependencies

| Criterion | Review outcome |
| --- | --- |
| Independent | Each story has a distinct observable outcome; shared profile, corpus and event prerequisites are explicit. Recovery stories intentionally depend on the normal outcome/state they recover. |
| Negotiable | UI presentation and source/implementation choices remain negotiable within approved requirements; quantitative scope and confirmation/exclusion rules are not silently negotiable. |
| Valuable | Each statement links a user action to deciding meals, keeping stock accurate or recovering safely; technology-only obligations are kept in Q-01–Q-06. |
| Estimable | Inputs, outcomes and dependencies are specified. Source/model feasibility remains a known checkpoint, so effort estimates are not invented. |
| Small | Feature stories are split by outcome; import recovery, constraint relaxation, stale refresh, shortage correction, undo and retries are separated when distinct user actions exist. US-06 remains a whole-plan user outcome while scaling and shopping are separate. |
| Testable | Every story has stable numbered Given/When/Then scenarios. Concrete arithmetic and stock values anchor state behavior; cross-cutting performance/agent targets remain measurable obligations. |

Review corrections applied: separated recovery actions from passive error handling; isolated substitution approval; made source eligibility reusable; preserved no-auto-save and no-stock-change boundaries; avoided converting personas into extra accounts. Residual dependencies are documented above, not eliminated by assertion. No approved requirement is intentionally omitted and no extra MVP capability is introduced.

## Extension compliance and review gate

Security and Resiliency extensions remain disabled; inherited explicit requirements remain binding. For this User Stories artifact stage, PBT-01 (Functional Design identification), PBT-02 (round trips), PBT-03 (invariants), PBT-04 (idempotency), PBT-05 (oracles), PBT-06 (stateful testing), PBT-07 (generators), PBT-08 (shrinking/reproducibility/CI), PBT-09 (framework/dependency configuration), and PBT-10 (complementary tests) have N/A implementation verification under the extension stage table: no applicable design/code/test execution occurs here. Their future obligations are preserved in Q-06; Hypothesis remains the selected direction. No applicable blocking extension finding; no implemented compliance claimed.

Both story artifacts are explicitly approved; proceed only to Workflow Planning as requested. That approval does not approve its execution plan or authorize Construction.
