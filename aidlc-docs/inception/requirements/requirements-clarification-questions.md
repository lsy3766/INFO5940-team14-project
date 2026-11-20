# Requirements Clarification Questions — Round 2

Status: Awaiting team answers. No option is preselected. This file does not approve requirements or any later stage.

The original response file is preserved unchanged. No answer fields are empty there; Q5 and Q6 contain questions rather than complete decisions, Q10 omits requested detail, Q13 supplies roles but not the remaining review arrangements, and Q1 requires a revised baseline. Different recipe counts and leftover policies supersede draft proposals only after baseline clarification.

Fill each [Answer]: with a letter and necessary details. Choose X for a custom policy. All numerical targets below are proposals, not inferred team decisions.

## Question 1: Revised team baseline (original Q1 = B)

How will the revised baseline required by your Q1 answer be supplied?

A) The team will update the four input documents and report when they are ready; wait before drafting requirements.

B) Treat the original answers plus validated follow-up answers as the revised baseline, superseding conflicting draft text; preserve input files unchanged.

X) Other (please describe after [Answer]:).

[Answer]: B

## Question 2: Recipe retrieval route (original Q5)

Your requested minimum of 45 attributed Chinese and Western recipes is recorded. Which acquisition and retrieval scope should the requirements specify?

A) Local curated RAG plus supported public-URL imports; defer general live search. Investigate candidate datasets during Construction before selecting a source.

B) Make general live search an MVP requirement in addition to local RAG and URL imports; free provider feasibility must be resolved before promising this capability.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 3: Sources and incomplete recipes (original Q6)

Which source and data-quality policy should apply?

A) Use team-authored or explicitly permitted content with provenance; defer the specific source list to a Construction feasibility checkpoint; incomplete or unreviewed recipes cannot enter automatic plans or deductions until corrected and confirmed.

B) The team will supply source sites or datasets and intended use now; wait for source assessment before finalizing requirements.

X) Other (please describe after [Answer]:).

[Answer]:X :We don’t want to create original recipes ourselves. During the construction phase, I’d like you to build as large a recipe database as possible to serve as the source for our recipe RAG system.

## Question 4: Time and repetition (original Q10 = B)

Which default rule should be used, with exclusions always hard and explicit permission required to relax time or repetition?

A) Default 30 minutes per meal; no identical main dish on consecutive days; users can set meal-specific time limits.

B) Default 15 minutes for breakfast and 45 minutes for lunch/dinner; no identical main dish within the five-day plan; users can set meal-specific time limits.

X) Other (please describe after [Answer]:).

[Answer]:X：Default 15 minutes for breakfast and 45 minutes for lunch/dinner; no identical main dish on consecutive days; users can set meal-specific time limits.

## Question 5: Role assignments and course review (original Q13)

The five supplied role assignments are recorded exactly and do not grant approval authority. Which remaining review arrangement applies?

A) Use the five requested Inception stages as the currently known rubric baseline; meeting times remain unspecified; the workflow operator relays explicit team review outcomes and records disagreement; additional rubric may be supplied before final review.

B) The team will provide a rubric and review/approval arrangement now; wait before finalizing requirements.

X) Other (please describe after [Answer]:).

[Answer]:A: but the meeting times could be setting on every Friday for 2 hours.

## Question 6: Agent evaluation and plan usability

Which measurable future evaluation target should supplement the original Q12 acceptance targets?

A) Use 10 fixed scenarios: at least 8 produce a usable valid plan or correct infeasibility explanation; all 10 preserve hard exclusions and write-confirmation boundaries; inspect tool traces for retrieval, inventory access, deterministic validation, and revision when needed; user can save a feasible plan with at most one requested revision.

B) The team will supply its own scenario count, success threshold, tool-use checks, and usability target before requirements are finalized.

X) Other (please describe after [Answer]:).

[Answer]: A

## Question 7: Property-based testing framework

Full PBT was enabled in original Q16. Which framework direction should be carried into later design (no installation, tests, or CI changes now)?

A) Plan Hypothesis with pytest for Python; retain example-based tests and include later stateful tests, shrinking, reproducibility, and CI coverage as required by the enabled rules.

B) Keep full PBT enabled but provide another Python-compatible framework and rationale before technical design.

X) Other (please describe after [Answer]:).

[Answer]: A

## Interpretation notes — not additional decisions

- Original Q8 A: plans may show shortages, but confirmation cannot deduct unavailable stock; the user can correct inventory or quantities and obtain a fresh valid preview. Undo is not deletion of history and must not be applied twice.
- Original Q9 B: takeout and leftover meals are annotations only; there is no leftover portion inventory. Original cooking is deducted once; eating its leftovers does not deduct the original raw ingredients again.
- Original Q11 A: direct manual Save/Add/Edit/Delete actions confirm those edits; agent writes and cooking deductions still require preview and explicit confirmation.
- Original Q12 A supplies correctness and latency targets, not evidence that any implementation meets them.
- Original Q14 B and Q15 B disable extension rule sets, not the explicit security and error-handling requirements already in the input drafts.
- Original Q16 A enables full PBT. Construction-specific PBT implementation checks are not yet applicable in this Inception-only run.
