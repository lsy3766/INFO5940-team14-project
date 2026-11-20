# Cooking Assistant — Personas

Status: Explicitly approved by the team, recorded at 2026-11-20T19:27:57Z. Subsequent artifacts require separate approval.

These are illustrative design archetypes, not interview findings or real participants. No demographic, biography, or research claim is implied. Both describe alternative contexts within one local profile and inventory, without login or concurrent editing.

## P-01 — Solo-focused busy cook

- Goal: Decide what to eat over five days without repeatedly calculating stock and shopping needs.
- Context: Usually prepares one serving, with 15-minute breakfasts and 45-minute lunches/dinners unless overridden.
- Pain points: Decision fatigue, recipes incompatible with exclusions or time, forgotten stock changes when plans change.
- Needs: Attributed usable recipes, a flexible 15-slot plan, deterministic shopping arithmetic, clear errors and editable consumption previews.
- Constraints: One local profile; free model access is conditional; cannot assume search access or offline hosted-model generation; hard exclusions remain enforced.
- Success: Can save a feasible plan with at most one requested revision, adapt meals, and confirm actual consumption without unintended or repeated deductions.
- Relevant stories: US-01–US-19; particularly setup, planning, shopping, meal changes, and recovery.

## P-02 — Occasional shared-meal cook

- Goal: Adapt the same local plan when preparing shared dishes for 2–10 diners.
- Context: Changes meal-specific servings, combines dishes without assuming one dish per person, and sometimes records leftovers or takeout.
- Pain points: Incorrect scaling, double-counted shopping stock, stale previews after inventory corrections, deductions repeated when leftovers are eaten.
- Needs: Per-meal overrides, compatible-unit arithmetic, preview correction, safe confirmation/undo, and annotation-only leftover handling.
- Constraints: Common appetite assumption; no per-person appetite model, additional profile, guest account, household collaboration, or leftover portion inventory.
- Success: Shared-dish quantities and shopping shortages are correct; actual cooking is deducted once; changed plans and leftovers do not silently consume stock.
- Relevant stories: US-01–US-19; particularly US-04, US-07–US-09 and US-11–US-18.

## Mapping rationale

Both archetypes may use all supported features. Stories reference both where behavior is shared; P-02 examples exercise serving overrides without adding capabilities. Persona success statements inherit requirements AC-01–AC-08; they are not observed user-study results. See stories.md for explicit scenarios, dependencies and coverage.
