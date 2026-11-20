# Cooking Assistant — Requirements

Status: Approved by explicit team statement in chat, recorded at 2026-11-20T19:08:16Z. Approval covers this requirements document only; later artifacts require separate approval.

## 1. Intent and baseline

- User request: Complete the course mid-semester Inception deliverables using AI-DLC v1.0.1: Requirements Analysis, User Stories, Workflow Planning, Application Design, and Units Generation. Stop after explicit Units Generation approval; do not enter Construction or implement application code in this run.
- Request type: New project (Greenfield).
- Scope: System-wide, multiple components: local user interface, recipe retrieval, agent tools, meal planning, inventory, and cooking-event bookkeeping.
- Complexity: Complex; comprehensive requirements depth due to deterministic state changes, model integration, and five stakeholders.
- Primary benefit: Help a busy individual decide what to eat and keep a five-day plan, shopping needs, and stock consistent when actual meals change; persistent validated state and explicit tool execution distinguish this from an unconstrained chatbot.

Inputs: README.md, docs/inputs/vision.md, docs/inputs/technical-environment.md, docs/COLLABORATION.md. R1 means requirement-verification-questions.md; R2 means requirements-clarification-questions.md; R3 means requirements-clarification-round-3-questions.md, all in this directory. Confirmed answers supersede conflicting draft text (R2 Q1 B). Original input files remain unchanged. No past meeting, approval, implemented feature, or measured result is inferred.

Consistency result: All 25 answer fields across three rounds are populated. R3 Q1 A and Q2 A resolve the last product-requirements blockers. No unresolved substantive conflict prevents drafting requirements. Specific source eligibility, supported sites, model access, and pinned dependencies remain explicit future feasibility checks, not promises of availability.

## 2. Users and operating scope

One local profile with one household inventory, without login or concurrent editing (R1 Q2 A, Q3 A). The primary user cooks for themselves or others; party size is an integer from 1 through 10, with meal-specific overrides, a common appetite assumption, and shared dishes. There is no one-dish-per-person rule or per-person appetite model (R1 Q4 A).

The UI is English and keyboard-operable (R1 Q12 A). Discussion and team review use Chinese; deliverables use English.

## 3. Functional requirements

| ID | Requirement | Basis |
| --- | --- | --- |
| FR-01 | Record and edit preferences, dietary exclusions, party size, and meal-specific cooking time and serving overrides. Reject invalid serving counts. | Vision; R1 Q4, Q10; R2 Q4 |
| FR-02 | Maintain recipe records with stable identity, title, ingredients and quantities, compatible units, base servings, instructions, meal tags, preparation-time estimates, source provenance and retrieval date. | Technical input; R3 Q2 |
| FR-03 | Provide at least 45 eligible, complete, distinct recipes with Chinese and Western coverage across breakfast, lunch, and dinner for MVP acceptance. Investigate existing datasets and supported public pages during Construction. Expand beyond 45 where suitable sources permit, with no guaranteed additional volume or MVP-blocking expansion target. Do not require original recipe authoring. | R1 Q5; R2 Q3; R3 Q1 A |
| FR-04 | Retain provenance and include only content whose terms permit intended use. Keep incomplete or unreviewed records out of automatic planning and deductions until missing information is supplied and confirmed. Report source or coverage shortfalls; never invent data, permissions, or sources. The collection-size floor counts only eligible, complete, distinct records. | R3 Q2 A |
| FR-05 | Retrieve local recipes using ingredient/tag filters and keyword ranking, and provide selected recipe information to the model before generation. Embeddings and a vector database are not required. Import user-provided public URLs only from supported sources, parsing Recipe JSON-LD where available and validating fields. For blocked, unsupported, or incomplete pages, show clear errors and permit pasted source text, subject to the same provenance and completeness policy. | Technical input; R2 Q2 A; R3 Q2 A |
| FR-06 | Allow manual inventory additions, corrections, and removals. Represent nonnegative quantities with units. Treat a direct manual Save/Add/Edit/Delete action as user confirmation of that edit. | Vision; R1 Q7 A, Q11 A |
| FR-07 | Generate a personalized five-day plan with 15 meal slots: breakfast, lunch, and dinner each day. Propose combinations of dishes and quantities appropriate to party size, preferences, time, and available inventory. Plans may expose shortages for shopping; stock sufficiency is not required merely to suggest a meal. | Vision; R1 Q4, Q8, Q12 |
| FR-08 | Enforce exclusions as hard constraints. Default time budgets are 15 minutes for breakfast and 45 for lunch/dinner; users can override per meal. Avoid an identical main dish on consecutive days. Time and repetition constraints may be relaxed only with explicit user permission; exclusions must not be silently relaxed. Explain infeasibility or insufficient candidates instead of claiming success. | R1 Q10 B; R2 Q4 X |
| FR-09 | Let users replace, skip, or adjust meals and servings at any time. Planning and plan revisions do not consume inventory. Do not auto-save plan revisions; preview agent-proposed persistent changes and require explicit confirmation. | Vision; R1 Q11 A |
| FR-10 | Scale ingredient requirements using deterministic logic and aggregate shopping needs across planned dishes, subtracting available inventory without repeatedly crediting the same stock. Support g/kg, ml/l, and count; convert only within compatible dimensions. Do not assume ingredient density, count-to-weight conversion, or household-measure equivalence. Substitutions require a user-approved recipe variant. Flag unsupported or incompatible units instead of fabricating arithmetic. | Technical input; R1 Q7 A |
| FR-11 | Let users select a dish actually cooked from the plan or recipe library, preview estimated ingredient usage, and correct actual quantities. Preview/cancellation must not change stock. Explicit confirmation is required before deduction. | Vision; R1 Q8 A, Q11 A |
| FR-12 | Revalidate stock and preview freshness at confirmation. Reject stale previews and insufficient stock; require correction or restocking and a fresh valid preview. Commit deductions atomically, never produce negative stock, and prevent duplicate deductions using idempotency identifiers. A repeated confirmation must not repeat the effect. | Technical input; R1 Q8 A |
| FR-13 | Support undo as a validated compensating transaction, once per original deduction; preserve event history and reject unsafe or repeated undo. Recheck remaining meals against current inventory and propose adjustments for user confirmation rather than silently rewriting the plan. | Vision; R1 Q8 A |
| FR-14 | Record takeout and leftovers as meal annotations only, without a leftover-portion inventory. These annotations do not deduct raw ingredients. Original cooking is recorded once; consuming leftovers must not deduct its raw ingredients again. Corrected actual consumption governs the cooking deduction rather than how much was eaten immediately. | R1 Q9 B; technical input |
| FR-15 | The agent uses explicit allowlisted tools to read preferences/inventory, retrieve recipes, validate proposals, and revise them based on tool results. Calculations, exclusions validation, stock writes, and confirmation checks are enforced by deterministic backend logic, not model promises. Never claim tool success without results. | Vision; technical input; R2 Q6 A |
| FR-16 | On model quota/access failures, retrieval errors, or infeasible requests, report the limitation with a clear recovery path. Use bounded requests; never silently switch to paid services, enable billing, retry indefinitely, or treat model knowledge as a verified recipe source. | Technical input; R1 Q12 A |

## 4. Non-functional requirements and technical constraints

| ID | Requirement | Basis |
| --- | --- | --- |
| NFR-01 | Run locally on team laptops with UI/backend bound to localhost. Store persistent application data in SQLite. No cloud hosting or public exposure required. | Technical input |
| NFR-02 | Baseline: Python 3.12.x, venv/pip, FastAPI, Pydantic v2, Streamlit, sqlite3, HTTPX, Beautiful Soup 4, pytest and FastAPI TestClient. Use a direct provider SDK and small explicit tool loop; no mandatory orchestration framework. Pin compatible dependency releases only at Construction start. | Technical input |
| NFR-03 | Planned product provider/model remains Gemini Developer API gemini-3.5-flash-lite, free tier only, subject to account, quota, function-calling, and availability checks during Construction. It is a planning baseline, not a verified working connection. Ollama is only a possible future alternative requiring separate feasibility testing. | Technical input |
| NFR-04 | Target completion of non-model local actions within 2 seconds; model requests must time out within 60 seconds with a clear error and retry action. Validate these targets on the implementation and demo workload later; no timing results are claimed now. | R1 Q12 A |
| NFR-05 | Validate tool inputs, exclusions, finite quantities, serving bounds, and unit compatibility. Keep API keys server-side in ignored local configuration; never commit or log keys. Use fictional demo profiles. Treat imported/web text as untrusted data, not instructions. | Technical input |
| NFR-06 | URL retrieval accepts public HTTP(S) only, rejects private/loopback destinations and unsafe redirects, and bounds size and time. Do not execute arbitrary URL content or model-generated code. Do not use pickle for untrusted imports, unsafe YAML loaders, or eval/exec of model output; use validated JSON and allowlisted functions. | Technical input |
| NFR-07 | Preserve stock and event integrity through transactions, inventory version checks, idempotency, and validated undo. Keep deterministic logic separately testable from model behavior using fake tool/model responses for most tests. | Technical input; R1 Q8, Q12 |
| NFR-08 | Use full property-based testing with Hypothesis and pytest in later Construction, complementary to example-based tests. Carry property identification, round trips, invariants, idempotency, oracle comparison where applicable, stateful sequences, domain-specific generators, shrinking, seed-based reproduction, and CI coverage into applicable design/test plans. No tests or dependency installation are authorized now. | R1 Q16 A; R2 Q7 A; PBT-01–PBT-10 |
| NFR-09 | Provide clear editable previews, validation errors, source labels, shortage explanations, and keyboard-operable English interactions. Do not guarantee allergy safety, infer spoilage, or give medical nutrition advice. | Technical input; R1 Q12 A |

## 5. Acceptance criteria (future implementation validation)

These are required outcomes, not completed tests or measured results.

| ID | Acceptance evidence | Requirements |
| --- | --- | --- |
| AC-01 | A valid request produces a five-day, 15-slot plan or a correct infeasibility explanation; exclusions hold and time/repetition exceptions require explicit permission. Party-size boundaries 1 and 10 are accepted; invalid counts are rejected; shared dishes and meal overrides work. | FR-01, FR-07, FR-08 |
| AC-02 | Count at least 45 distinct eligible complete recipes; demonstrate both cuisine groups and all three meal types. Each counted record has required fields and provenance. Incomplete/unreviewed imports never enter automatic planning or deductions. Unsupported retrieval returns an actionable error. | FR-02–FR-05 |
| AC-03 | Example tests and generated properties verify serving arithmetic, compatible conversions, exclusion filters, shopping aggregation and nonnegative shortages; incompatible units do not silently convert. | FR-08, FR-10; NFR-08 |
| AC-04 | Integration tests demonstrate plan modification, skip, quantity edits, preview cancellation, and proposal generation with zero inventory mutation. Manual save confirms its own edit; agent writes and cooking deductions require preview and explicit confirmation. | FR-06, FR-09, FR-11 |
| AC-05 | Integration tests show exactly one deduction for repeated confirmation, rejection of stale/insufficient stock, no negative stock, safe once-only undo with history, and confirmed proposals for remaining meals. Takeout/leftovers annotations never cause a second raw-ingredient deduction. | FR-12–FR-14 |
| AC-06 | Retrieval failure and model quota/access failure produce clear errors, bounded behavior, and no hidden paid fallback or unintended writes. Verify local-action 2-second target and 60-second model timeout, plus keyboard use of core flows. | FR-16; NFR-01, NFR-03–NFR-06, NFR-09 |
| AC-07 | On 10 fixed scenarios, at least 8 yield a usable valid plan or correct infeasibility explanation; all 10 preserve hard exclusions and write-confirmation boundaries. Inspect tool traces for retrieval, inventory access, deterministic validation, and revision where needed. A feasible plan can be saved with at most one requested revision. | R2 Q6 A; FR-15 |
| AC-08 | Test strategy contains example-based critical-path tests plus full applicable PBT obligations, including domain generators, shrinking, reproducible seeds and CI execution. Stateful stock sequences check invariants after each operation. No live model access is required for most tests; live agent evaluation is deferred to Construction. | NFR-07, NFR-08 |

## 6. Boundaries and deferred feasibility

Out of MVP scope: multiple profiles/login, concurrent editing, cookware matching, per-person appetite models, leftover portion inventory, general live web search, supermarket pricing, purchasing/payments, photo recognition, medical nutrition advice, and cloud deployment. No original recipe creation is required; model-invented recipes cannot satisfy attributed corpus acceptance.

Local recipe retrieval and access can work without internet once data exists; hosted-model generation is not promised offline. Supported URL import is not universal website access. Internet access and dataset existence do not establish permission or data quality.

Construction checkpoints (not current blockers to this document): inspect candidate dataset/page terms and field completeness, establish supported URL sources, acquire and validate the 45-recipe floor, verify free model access/tool calling and quotas, pin dependencies, and measure implementation performance. If feasibility fails, report it and seek team scope/provider decisions rather than silently weakening requirements or assuming a fallback. Dataset acquisition, API testing, implementation, and test execution are not performed in this Inception run.

## 7. Changes from original drafts

| Topic | Confirmed clarification |
| --- | --- |
| Audience/access | Busy individual is the priority scenario; one local profile and inventory; no login or concurrency. |
| Servings | Bound to 1–10 with shared dishes and meal overrides; common appetite, no per-person appetite model. |
| Corpus | Minimum 45 eligible complete distinct recipes, Chinese and Western coverage, all meal types; existing-source acquisition rather than original authoring. Expansion above the floor is optional and unbounded in target, not an acceptance blocker. |
| Retrieval | Local RAG plus supported public URLs is required; general live search is deferred rather than a conditional MVP promise. |
| Data quality | Explicit source-use eligibility and completeness gates; incomplete/unreviewed records excluded from automatic plans and deductions. |
| Time/variety | 15/45/45-minute defaults; no identical main dish on consecutive days; permission before relaxing time/repetition, never silent relaxation of exclusions. |
| Units | g/kg, ml/l and count only; no assumed density or household measures; approved recipe variants for substitutions. |
| Transactions | Explicit insufficient/stale-stock rejection, exactly-once deduction and safe once-only compensating undo. |
| Leftovers | Annotation-only; no portion inventory or repeated raw-ingredient deduction. |
| Confirmation | Direct manual save/edit confirms that edit; agent writes and cooking deductions require preview and explicit confirmation; no automatic saving of plan revisions. |
| Evaluation | Numeric timing and 10-scenario/8-success targets; all scenarios protect exclusions/confirmation; full PBT with Hypothesis complements example tests. |
| Collaboration | Named responsibilities supplied; Friday two-hour meeting remains a proposal, not a finalized schedule or historical meeting. |

## 8. Governance and course deliverables

All five requested Inception stages are mandatory for this course run even where the generic workflow labels a stage conditional. Requirements approval does not approve later artifacts automatically. Next stage after explicit requirements approval is User Stories, including its planning and artifact review gates. Stop after Units Generation is reviewed and explicitly approved; no Construction.

| Responsibility | Reported member |
| --- | --- |
| Workflow & GitHub | Shuyue Li |
| Product & Requirements | Shining Cao |
| UX & User Stories | Danying Wei |
| Technical & LLM | Yuyi Long |
| Evaluation & Delivery | Yichen Tong |

The operator relays explicit team review outcomes and records disagreement, without approving for absent teammates. Use the five requested stages as the known milestone baseline; any additional rubric is supplied before final review. Meeting cadence/time is not an application requirement and remains tentative. No meetings, answers, or approvals before the actual recorded workflow are invented.

## 9. Extension compliance at Requirements Analysis

| Rule | Status at this stage | Rationale / carried obligation |
| --- | --- | --- |
| Security Baseline | Disabled | R1 Q14 B; explicit input security requirements remain in NFR-05/06. |
| Resiliency Baseline | Disabled | R1 Q15 B; explicit integrity/error requirements remain. |
| PBT-01 | N/A | Functional Design-specific property identification deferred; carry forward via NFR-08. |
| PBT-02 | N/A | No serialization implementation/tests generated; round-trip obligation retained. |
| PBT-03 | N/A | No code/test generation; arithmetic and stock invariants retained. |
| PBT-04 | N/A | No implementation; idempotency testing obligation retained. |
| PBT-05 | N/A | No implementation/oracle comparison at this stage; evaluate when designing tests. |
| PBT-06 | N/A | Stateful testing implementation deferred; inventory sequence testing retained. |
| PBT-07 | N/A | No generators implemented; domain generator obligation retained. |
| PBT-08 | N/A | No test execution/CI implementation now; shrinking, seeds and CI obligations retained. |
| PBT-09 | N/A | NFR Requirements/implementation checks occur later; Hypothesis direction is already recorded without dependency installation. |
| PBT-10 | N/A | No test generation; complementary critical-path example tests retained. |

No applicable extension blocking findings at this stage. This does not assert Construction compliance or successful tests.

## 10. Review gate

Team requirements approval explicitly received and recorded at 2026-11-20T19:08:16Z. Continue to User Stories planning only, with separate plan and artifact gates. No later-stage artifact is approved by this document.
