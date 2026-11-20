# Requirements Clarification Questions — Round 3

Status: Awaiting team answers. Neither option is preselected. No requirements approval is implied.

## Consistency review

All 16 Round 1 and seven Round 2 answer fields contain text. Round 2 Q1 B establishes the answers as the revised baseline, superseding conflicting input drafts without editing those drafts. Round 2 Q2 A confirms local curated RAG, supported public-URL imports, and deferral of general live search. Round 2 Q4 specifies 15 minutes for breakfast and 45 minutes for lunch/dinner, no identical main dish on consecutive days, and meal-specific overrides; original Q10 B retains explicit permission before relaxing time or repetition. Round 2 Q6 and Q7 confirm evaluation targets and Hypothesis with pytest.

Round 2 Q3 rejects authoring original recipes and requests the largest possible database during Construction. This is a goal, not a bounded acceptance criterion or a decision about source eligibility and incomplete imports. The two questions below are the remaining product-requirements blockers.

Round 2 Q5 reports a possible weekly Friday two-hour meeting. This differs from the input draft's twice-weekly arrangement and remains tentative; no specific time or final cadence is inferred. It is not a product-requirements blocker. Recorded role assignments remain unchanged and do not grant authority to approve for absent members.

## Question 1: Recipe corpus size and completion criterion

How should “as large a recipe database as possible” relate to the previously requested minimum of 45 attributed Chinese and Western recipes across all three meal types? These are proposed alternatives, not selected targets.

A) Retain at least 45 eligible, complete, distinct recipes as the MVP acceptance floor. Expand further using suitable existing sources during Construction, but additional volume has no guaranteed target and does not block MVP completion. No original recipe authoring is required.

B) A larger guaranteed corpus is essential for MVP acceptance. Specify the minimum eligible, complete, distinct recipe count, required coverage, and effort or time limit after [Answer]:; confirm source feasibility during Construction before accepting a size promise. No original recipe authoring is required.

X) Other (provide the desired minimum, coverage, and stopping rule after [Answer]:).

[Answer]: A

## Question 2: Existing-source eligibility and incomplete records

The source list can be investigated during Construction, as selected in Round 2 Q2 A. Which quality and usage policy should govern acquisition? For example, a retrieved recipe without ingredient quantities or base servings cannot yet support reliable serving arithmetic or stock deductions.

A) Investigate existing datasets and supported public recipe pages during Construction. Include only content whose terms permit the intended use, retaining provenance. Keep incomplete or unreviewed records out of automatic plans and deductions until missing information is supplied and confirmed. Report source or coverage shortfalls rather than fabricate data, sources, or permissions. Do not require the team to author original recipes.

B) Apply the same source-permission and provenance checks, but permit incomplete records to appear as clearly labelled discovery-only results; exclude them from automatic plans, shopping arithmetic, and deductions until corrected and confirmed. Report source or coverage shortfalls; do not require original recipe authoring.

X) Other (describe the intended source-selection and missing-data policy after [Answer]:).

[Answer]: A
