# Unit of Work Story and Requirement Map

Status: Explicitly approved by the team, recorded at 2026-11-20T19:58:21Z. Units Generation and Inception are complete. STOP before Construction. Documentation only; no application or test files created.

## Assignment rules

All stories belong to UOW-01. Each row has exactly one primary business module accountable for the user outcome plus supporting collaborators; these are responsibility mappings, not exclusive member assignments or independently deployed releases. Both P-01 and P-02 contexts remain supported. All original numbered acceptance scenarios remain binding; no scenario is removed or claimed tested.

US-19 has M04 as primary owner for the planning/revision service-recovery outcome; M02 owns recipe-import recovery in US-05 and collaborates on shared error handling. This avoids an unowned generic failure story while preserving the approved cross-capability error contract. Supporting M06/M08/M09/M10 responsibilities do not change model or persistence authority.

| Story | Outcome | Unit | Primary module | Collaborators | Original scenario coverage |
| --- | --- | --- | --- | --- | --- |
| US-01 | Set preferences and meal constraints | UOW-01 | M01 | M06, M07, M10 | US-01-AC1, US-01-AC2, US-01-AC3 |
| US-02 | Maintain inventory | UOW-01 | M03 | M06, M07, M10 | US-02-AC1, US-02-AC2, US-02-AC3 |
| US-03 | Find attributed usable recipes | UOW-01 | M02 | M06, M07, M10 | US-03-AC1, US-03-AC2, US-03-AC3 |
| US-04 | Import an existing public recipe | UOW-01 | M02 | M06, M07, M09, M10 | US-04-AC1, US-04-AC2, US-04-AC3 |
| US-05 | Recover a failed import with source text | UOW-01 | M02 | M06, M07, M09, M10 | US-05-AC1, US-05-AC2 |
| US-06 | Generate a five-day plan | UOW-01 | M04 | M01, M02, M03, M06, M07, M08, M09, M10 | US-06-AC1, US-06-AC2, US-06-AC3 |
| US-07 | Relax a feasible soft constraint explicitly | UOW-01 | M04 | M01, M02, M06, M07, M08, M09 | US-07-AC1, US-07-AC2, US-07-AC3 |
| US-08 | Adjust shared dishes and scale servings | UOW-01 | M04 | M01, M02, M06, M07 | US-08-AC1, US-08-AC2, US-08-AC3 |
| US-09 | View an inventory-aware shopping list | UOW-01 | M04 | M02, M03, M06, M07 | US-09-AC1, US-09-AC2, US-09-AC3 |
| US-10 | Replace, skip and save meal changes | UOW-01 | M04 | M01, M02, M06, M07, M10 | US-10-AC1, US-10-AC2, US-10-AC3 |
| US-11 | Preview actual cooking quantities | UOW-01 | M05 | M02, M03, M06, M07, M10 | US-11-AC1, US-11-AC2, US-11-AC3 |
| US-12 | Confirm cooking exactly once | UOW-01 | M05 | M02, M03, M06, M07, M10 | US-12-AC1, US-12-AC2, US-12-AC3 |
| US-13 | Recover a stale cooking preview | UOW-01 | M05 | M03, M06, M07, M10 | US-13-AC1, US-13-AC2 |
| US-14 | Resolve insufficient cooking stock | UOW-01 | M05 | M02, M03, M06, M07, M10 | US-14-AC1, US-14-AC2 |
| US-15 | Undo a cooking deduction safely | UOW-01 | M05 | M03, M06, M07, M10 | US-15-AC1, US-15-AC2, US-15-AC3 |
| US-16 | Review remaining-meal adjustments | UOW-01 | M04 | M01, M02, M03, M06, M07, M08, M09, M10 | US-16-AC1, US-16-AC2 |
| US-17 | Record takeout and leftovers | UOW-01 | M04 | M05, M06, M10 | US-17-AC1, US-17-AC2, US-17-AC3 |
| US-18 | Approve a substitution variant | UOW-01 | M02 | M06, M07, M10 | US-18-AC1, US-18-AC2 |
| US-19 | Recover from model or tool failure | UOW-01 | M04 | M02, M06, M07, M08, M09, M10 | US-19-AC1, US-19-AC2, US-19-AC3 |

Recovery dependencies remain exactly those documented in stories.md: import correction follows import failure; relaxation follows infeasibility; stale/shortage recovery follows a preview; undo follows a committed event; service retry follows a failed operation. Primary assignment does not erase these dependencies or create inter-unit calls.

## Complete requirements and acceptance mapping

UOW-01 is accountable for every row. Evidence below is carried from approved application-design.md; translate component IDs to modules using unit-of-work-dependency.md. Cross-cutting constraints are obligations on the entire unit, not invented user features or separately deployed units.

| Requirement | Accountable unit | Approved design evidence / module responsibilities |
| --- | --- | --- |
| FR-01 | UOW-01 | C02/C07: preferences and diner/time overrides |
| FR-02 | UOW-01 | C03 RecipeRecord contract and typed persistence |
| FR-03 | UOW-01 | C03 assess_coverage; 45 eligible complete distinct records, Chinese/Western and three meal types; optional expansion |
| FR-04 | UOW-01 | C03 eligibility/provenance/review; blocked automatic use for incomplete/unreviewed records |
| FR-05 | UOW-01 | C03 local filters/ranking and C09 supported URL/JSON-LD; pasted recovery |
| FR-06 | UOW-01 | C04 explicit manual edits via C10 |
| FR-07 | UOW-01 | C08/C05 validated 15-slot proposal and shared dishes |
| FR-08 | UOW-01 | C07 hard exclusions, 15/45/45 defaults, no consecutive-day same main dish; explicit soft permissions |
| FR-09 | UOW-01 | C05 draft changes/save; no auto-save or stock deduction |
| FR-10 | UOW-01 | C07 scale/aggregate, compatible g/kg ml/l count only; C03 approved variants |
| FR-11 | UOW-01 | C06 corrected actual-consumption preview and explicit confirmation |
| FR-12 | UOW-01 | C10 atomic_confirm with freshness, stock sufficiency and idempotent recorded outcomes |
| FR-13 | UOW-01 | C06/C10 safe once-only compensation; C08 remaining-meal proposals |
| FR-14 | UOW-01 | C05 annotations only; cooking consumption belongs to C06, no leftover stock |
| FR-15 | UOW-01 | C08 allowlisted read/compute loop; C07 validation and C06/C10 guarded writes |
| FR-16 | UOW-01 | C01 central errors; C08 bounded loop and C09 provider failures, no paid fallback |
| NFR-01 | UOW-01 | C01 localhost; C10 local SQLite, no public/cloud deployment |
| NFR-02 | UOW-01 | Approved Python/FastAPI/Pydantic/Streamlit/sqlite3/HTTPX/BS4/provider SDK/pytest stack retained; pins deferred |
| NFR-03 | UOW-01 | C09 free-tier Gemini baseline, access/tool quotas unverified until Construction; no assumed fallback |
| NFR-04 | UOW-01 | C01 final request state: 2-second local target, C08/C09 60-second model timeout |
| NFR-05 | UOW-01 | C01/C07 validated input; C09 server-side ignored keys, fictional demos, untrusted data boundaries |
| NFR-06 | UOW-01 | C09 public destination and redirect/size/time bounds; validated JSON, no unsafe deserialization/eval |
| NFR-07 | UOW-01 | C10 atomicity/version/idempotency/compensation; pure C07 and injectable C09 enable fakes |
| NFR-08 | UOW-01 | Future verification inventory below preserves full Hypothesis/pytest obligations |
| NFR-09 | UOW-01 | C01 keyboard-operable English previews/errors/source labels; no allergy/spoilage/medical guarantees |
| AC-01 | UOW-01 | C02/C05/C07/C08 diner boundaries, 15-slot/constraint or infeasibility validation |
| AC-02 | UOW-01 | C03/C09 coverage/eligibility and retrieval error contracts |
| AC-03 | UOW-01 | C07 scale/aggregate/filter; future example/property verification |
| AC-04 | UOW-01 | C04 direct-save confirmation; C05 non-consuming changes; C06 previews/cancel |
| AC-05 | UOW-01 | C06/C10 confirm/replay/stale/shortage/undo, C08 remaining meals, C05 annotations |
| AC-06 | UOW-01 | C01/C09 timing/accessibility/safety/failure, no paid fallback |
| AC-07 | UOW-01 | C08 PlanningTrace: 10 scenarios, 8 valid/infeasible success outcomes, all 10 protect exclusions/confirmation, at most one requested revision |
| AC-08 | UOW-01 | Pure C07, injectable adapters and C10 operation sequences support fake/integration/example/PBT tests later |

## Cross-cutting quality ownership

| Story quality obligation | Coordinating modules | Required future evidence, not present results |
| --- | --- | --- |
| Q-01 local stack | M06, M10, all consumers | Approved Python/local UI/API/SQLite stack, compatible pins later, localhost only, no cloud requirement |
| Q-02 free model/data feasibility | M02, M08, M09 | Permitted complete attributed corpus of at least 45; both cuisine groups/all meals; free-tier model and native tool checks; no paid fallback or fabricated source |
| Q-03 input/fetch safety | M06, M07, M09, M10 | Finite supported quantities, diner limits, server-side secrets, public-URL/redirect/size/time guards, no unsafe serialization/eval; untrusted content cannot authorize writes |
| Q-04 usability/performance | M06 with all invoked modules | English keyboard core flows, editable previews, clear errors/sources; local-action 2-second target, model 60-second timeout; no medical/allergy/spoilage guarantees |
| Q-05 agent evaluation | M04, M08, M07, M09 | 10 fixed scenarios: at least 8 usable valid plan/correct infeasibility successes; all 10 preserve exclusions/confirmation; actual tool-trace inspection; feasible save within one requested revision |
| Q-06 tests/state integrity | M07, M10 and all workflow modules | Fake model/tools for most checks, example tests plus applicable full Hypothesis PBT, stateful reference model, structured generators/boundaries, shrinking, reproducible seeds and CI; live checks deferred |

Full-PBT obligations: PBT-01 property identification in future Functional Design; PBT-02 round trips; PBT-03 invariants; PBT-04 idempotency; PBT-05 oracle comparison where applicable; PBT-06 stateful sequences; PBT-07 domain generators; PBT-08 shrinking/reproducibility/CI; PBT-09 Hypothesis framework/dependency configuration; PBT-10 complementary example coverage. Assign properties to relevant modules in later design without claiming tests exist now.

## Coverage validation and extension applicability

19 unique story IDs are assigned once to UOW-01 with one primary business module each; all 52 scenario IDs are retained. All 16 FR, nine NFR, eight AC and six Q obligations are mapped. C01–C10 have corresponding M01–M10 boundaries. No orphan recovery story, invented feature, duplicate primary accountability or split stock transaction owner is introduced.

Security and Resiliency extensions are disabled; explicit approved safety/integrity obligations remain binding. PBT-01 implementation verification N/A here because formal property identification belongs to Functional Design; PBT-02–PBT-08/PBT-10 N/A because no code/tests/CI are generated or run; PBT-09 N/A because no NFR/dependency configuration stage occurs now. All individual future obligations are retained above. No applicable blocking extension findings; no runtime/test compliance claimed.

Review all three unit artifacts and explicitly approve or request changes. All three artifacts are explicitly approved. Units Generation and Inception are complete; STOP, not Construction.
