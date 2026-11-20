# Requirement Verification Questions

> English translation edition: extension questions 14–16 have been translated from Chinese without changing options or recorded answers. This is a language-only revision, not a new team decision.

Status: Awaiting team answers. No choices are selected or approved.

Fill every `[Answer]:` with a letter; use Other for custom decisions and include necessary details. Answers clarify scope and do not approve the subsequent requirements document. Proposed numbers and policies below are options, not established facts.

## Question 1: Draft baseline and decision authority

Which baseline may the workflow use, and how will team approval be communicated?

A) Use the four input drafts as a proposal; the operator will relay explicitly collected team answers and approvals, with dissent recorded.

B) Use the drafts only as background; provide a revised team baseline before requirements are drafted.

X) Other (please describe after [Answer]:).

[Answer]: B

## Question 2: Priority user and measurable benefit

Which primary scenario should drive the MVP and distinguish it from a general chatbot?

A) A busy individual: generate an inventory-aware five-day plan and keep shopping and stock consistent after meal changes.

B) A shared household: coordinate shared dishes and serving counts while keeping one common inventory consistent.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 3: Profiles and access

What is the local MVP user model?

A) One local profile and household inventory, without login or concurrent editing.

B) Multiple local profiles with separate preferences and inventories; specify sharing and access expectations.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 4: Servings and shared dishes

Which limits and appetite model should be supported?

A) Party size 1–10, default common appetite, meal-specific overrides, multiple shared dishes allowed; no per-person appetite model.

B) Party size 1–20, explicit small/standard/large appetite per meal and meal-specific overrides; shared dishes allowed.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 5: Recipe collection and scope

What initial recipe coverage and retrieval scope should be required?

A) At least 30 attributed recipes covering all meal types; Chinese home cooking emphasis; local RAG and supported public URLs required, general live search deferred.

B) At least 45 attributed recipes with Chinese and Western coverage across all meal types; local RAG and supported public URLs required, general live search deferred.

X) Other (please describe after [Answer]:).

[Answer]:X:At least 45 attributed recipes with Chinese and Western coverage across all meal types; Could you generate a rag directly or can you find some existing rag or data for us? Or is there any model can connect to internet?

## Question 6: Sources and missing recipe data

How should source permissions and incomplete imports be handled?

A) Use team-authored or explicitly permitted seed content with attribution; confirm source eligibility and supported URL sites later; block unreviewed or incomplete recipes from planning and deductions.

B) Provide a team-selected source list now, including permitted use and import rules, before finalizing retrieval requirements.

X) Other (please describe after [Answer]:).

[Answer]:X don't understand this question.

## Question 7: Units and substitutions

What ingredient normalization and substitution behavior is in scope?

A) Support g/kg, ml/l, and count; only convert within compatible dimensions; no density or count-to-weight assumptions; substitutions require a user-approved recipe variant.

B) Also support household measures via an explicit reviewed conversion table and ingredient-specific conversions; specify the table and substitution policy.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 8: Shortages, confirmation, and undo

Which inventory transaction policy should apply?

A) Allow shortage-aware plans and shopping lists, but block cooking confirmation on insufficient or stale stock; require correction or restock; prevent duplicate deductions; undo once only when a safe compensating transaction is valid.

B) Allow confirming actual partial consumption despite the original recipe shortage after a revised preview; block negative stock and stale previews; prevent duplicates; undo with a validated compensating transaction.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 9: Leftovers and takeout

Which bookkeeping depth is required?

A) Record takeout with no raw-ingredient deduction; record and consume leftover portions separately, tracking remaining portions without deducting raw ingredients twice.

B) Record takeout and leftovers only as meal annotations without portion inventory; neither deducts raw ingredients.

X) Other (please describe after [Answer]:).

[Answer]: B

## Question 10: Preferences and variety

What hard constraints and variety rules should apply?

A) Exclusions and per-meal time limits are hard constraints; prefer variety with no identical main dish on consecutive days; relax only soft preferences after explicit permission.

B) Exclusions are hard constraints; time and repetition are negotiable after explicit permission; provide exact variety goals and default time budget.

X) Other (please describe after [Answer]:).

[Answer]: B

## Question 11: Persistent writes and plan changes

How should the draft requirement that all persistent writes require confirmation apply to direct UI actions?

A) A direct Save/Add/Edit/Delete action is confirmation for manual data changes; agent-proposed writes and inventory deductions require an explicit preview and confirmation; plan revisions do not auto-save.

B) Every persistent write, including manual edits and plan revisions, requires a separate preview and confirmation step.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 12: Acceptance targets and quality

Which proposed acceptance baseline should be used for later Construction validation (no tests run now)?

A) Fifteen meal slots; all hard constraints validated; zero stock mutation during planning/preview; exactly-once confirmation and safe undo; deterministic tests for all listed failure cases; non-model local actions target 2 seconds and model requests time out within 60 seconds; clear errors and retry; keyboard-operable English UI.

B) Use the same correctness targets but provide different timing, UI language, accessibility, and agent-evaluation targets.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 13: Course review and collaboration

How should course coverage and unassigned roles be handled?

A) Use the five requested stages as the known milestone baseline; keep role assignments and exact meeting times unassigned unless the team supplies them; provide any additional rubric before final review.

B) Provide the course rubric, role assignments, and review arrangements now before requirements are finalized.

X) Other (please describe after [Answer]:).

[Answer]:1.Workflow & GitHub:Shuyue Li
2.Product & Requirements: Shining Cao
3.UX & User Stories: Danying Wei
4.Technical & LLM: Yuyi Long
5.Evaluation & Delivery: Yichen Tong

## Question 14: Security Baseline (Extension Selection)

Should the Security Baseline extension be enabled for this project?

A) Yes, enforce all security rules as blocking constraints (recommended for production applications).

B) No, skip the security extension (appropriate for proofs of concept, prototypes, or experiments); security requirements explicitly stated in the inputs still apply.

X) Other (please explain after [Answer]:).

[Answer]: B

## Question 15: Resiliency Baseline (Extension Selection)

Should the Resiliency Baseline be enabled? Based on AWS Well-Architected reliability principles, it provides 15 categories of design guidance covering business objectives, change management, observability, high availability, disaster recovery, and continuous improvement. It is only a design starting point, does not establish production readiness or guarantee availability, RTO, or RPO, and does not replace a formal architecture review.

A) Yes, apply it as directional best practices and design guidance, subject to further validation and strengthening.

B) No, skip the resiliency baseline (appropriate for prototypes and experiments).

X) Other (please explain after [Answer]:).

[Answer]: B

## Question 16: Property-Based Testing (Extension Selection)

Should the property-based testing extension be enabled? In this run it affects only requirements and design constraints; no tests will be written or run.

A) Yes, enforce all property-based testing rules as blocking constraints (recommended for projects with business logic, data transformations, serialization, or stateful components).

B) Enable it partially, covering only pure functions and serialization round trips.

C) No, skip the property-based testing extension (appropriate for simple CRUD, UI-only, or thin integration layers).

X) Other (please explain after [Answer]:).

[Answer]: A
