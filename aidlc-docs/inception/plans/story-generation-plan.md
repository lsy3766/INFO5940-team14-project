# Story Generation Plan

Status: Plan explicitly approved by the team, recorded at 2026-11-20T19:24:14Z. Generation completed; stories/personas explicitly approved by the team.

## Baseline and constraints

Use ../requirements/requirements.md as the approved product baseline. The team explicitly approved it and requested User Stories planning in the current chat. Do not reopen settled scope: one local profile, 1–10 diners, five days/15 slots, 45 eligible complete distinct recipes, local RAG and supported URL imports, no general live search, explicit cooking confirmation, safe once-only undo, annotation-only leftovers, zero-spend local operation, and existing acceptance targets.

Personas are illustrative design archetypes, not evidence from interviews. They do not imply extra accounts or permissions. Stories describe observable user outcomes, not endpoints, database tables, implementation tasks, sprint plans, or development timelines. All approved FR-01–FR-16, NFR-01–NFR-09, and AC-01–AC-08 must be traceable; cross-cutting constraints may be mapped as acceptance/quality obligations rather than artificial standalone user stories.

## Breakdown options

| Approach     | Benefits                                                                  | Trade-offs                                                  |
| ------------ | ------------------------------------------------------------------------- | ----------------------------------------------------------- |
| User journey | Connects setup, planning, shopping, cooking, and recovery in user context | Cross-cutting rules need explicit mapping                   |
| Feature      | Makes capability coverage easy to review                                  | Can fragment the end-to-end experience                      |
| Persona      | Highlights differing motivations                                          | Can duplicate stories and imply unsupported user roles      |
| Domain       | Groups recipe, inventory, planning, and cooking concepts                  | Can resemble internal architecture rather than user value   |
| Epic         | Supports broad themes with small stories                                  | Adds hierarchy; avoid using it as implementation scheduling |

Selected approach (Q1 B): feature-based groups with small outcome-based stories, a separate end-to-end journey, and a requirement coverage matrix. The earlier journey-based hybrid was an option only and is not the selected organization. Selection is validated and the plan is explicitly approved.

## Execution checklist

### Part 1 — Planning

- [x] Read approved requirements and relevant User Stories rules.
- [x] Record assessment explaining why stories add value.
- [x] Present breakdown alternatives and embedded questions.
- [x] Collect all answers below without selecting on the team's behalf.
- [x] Validate answers against requirements and one another; use a separate clarification file if necessary.
- [x] Incorporate validated choices into this plan, preserving the original answers in the audit.
- [x] Log the full finalized-plan approval prompt in audit.md.
- [x] Obtain explicit plan approval before generation.

## Finalized methodology (explicitly approved)

- Organization — Q1 B: Feature-based groups of small outcome-based stories, plus a separate end-to-end journey in stories.md covering setup, retrieval, planning, shopping, actual cooking, and recovery. Grouping is for review clarity, not a development sequence or sprint priority.
- Personas — Q2 B: Two illustrative archetypes: a solo-focused busy cook and a cook who occasionally prepares shared meals for 2–10 diners. These represent alternative contexts within the same single-local-profile product; they do not introduce concurrent users, accounts, new permissions, or claimed interview evidence.
- Story splitting — Q3 B: Separate normal-path stories from recovery stories when recovery requires a distinct observable user action. Link prerequisites and shared criteria explicitly. For example, stock correction/re-preview, confirmed constraint relaxation, and safe undo can have independent recovery outcomes; passive error messages stay with their relevant interaction rather than becoming artificial stories.
- Acceptance format — Q4 A: Numbered Given/When/Then scenarios, concrete inputs and observable outcomes for state and arithmetic; preserve approved performance and agent-evaluation targets in a linked cross-cutting section. No test execution or measured success is claimed.
- Traceability: Map all FR-01–FR-16, NFR-01–NFR-09 and AC-01–AC-08 to story scenarios or explicit cross-cutting obligations, map both personas to relevant stories, and review INVEST with honest dependency notes.
- Validation: Four fields contain valid, mutually consistent choices. Both archetypes fit the approved 1–10-diner model. Feature grouping is compatible with a separate journey; recovery splitting is compatible with numbered scenarios. No additional product-scope clarification is needed.

### Part 2 — Generation (only after plan approval)

- [x] Re-read the approved plan and requirements; identify the first incomplete generation step.
- [x] Generate ../user-stories/personas.md using the approved archetype choice, with goals, pain points, context, constraints, and success outcomes; label invented illustrative details as such and avoid unsupported demographics.
- [x] Generate ../user-stories/stories.md using the approved breakdown and format. Give each story a stable ID, persona reference, user-value statement, approved requirement references, preconditions, and observable acceptance criteria.
- [x] Cover preference/inventory setup, eligible recipe access/import, five-day planning, constraint permission/infeasibility, shopping arithmetic, plan changes, actual-cooking previews, confirmation, stale/insufficient stock, duplicate confirmation, undo, remaining-meal proposals, and takeout/leftover annotations.
- [x] Include clear normal, boundary, cancellation, and failure acceptance criteria as applicable. Preserve quantitative acceptance targets and distinguish proposed evaluation fixtures from validated real-user evidence.
- [x] Map personas to relevant stories and all FR/NFR/AC identifiers to story criteria or cross-cutting quality obligations; identify gaps without adding unapproved scope.
- [x] Check INVEST: independent value with explicit necessary prerequisites, negotiable presentation within fixed approved constraints, valuable outcomes, estimable behavior, small scope, and testable criteria. Record corrective edits and residual dependencies rather than falsely claiming all stories have no dependencies.
- [x] Verify consistency with approved requirements and extension applicability; update this plan's checkboxes in the same interaction as each completed step.
- [x] Present stories.md and personas.md for separate explicit artifact approval; do not continue to Workflow Planning until approved.

## Team decisions — answer all four questions

Use a letter after each [Answer]: and include details for X. These are story-method choices, not changes to product requirements. Providing answers does not itself approve this plan.

### Question 1: Story organization

Which breakdown should guide story generation?

A) Journey-based themes with small outcome-based stories and a requirement coverage matrix (the proposed hybrid).

B) Feature-based groups with small outcome-based stories, plus a separate end-to-end journey and requirement coverage matrix.

X) Other (describe the approach and clear grouping rules after [Answer]:).

[Answer]: B

### Question 2: Persona representation

How should the approved single-local-user audience be represented without inventing extra user accounts or research findings?

A) One primary illustrative busy-home-cook archetype; cooking for guests is a context variation within it.

B) Two illustrative archetypes within the same supported user model: a solo-focused busy cook and a cook who sometimes prepares shared meals for 2–10 people. They are alternative usage contexts, not simultaneous profiles or separate access roles.

X) Other (describe archetypes within the approved scope after [Answer]:).

[Answer]: B

### Question 3: Story size and recovery scenarios

How should normal and exceptional behavior be divided into stories?

A) One small story per distinct user outcome; put errors, boundaries, and cancellation in that story's acceptance scenarios unless recovery has independent user value (for example undo or revising an infeasible plan).

B) Separate normal-path and recovery stories where each recovery has a distinct observable user action; explicitly link dependencies and shared criteria to avoid duplicating requirements.

X) Other (describe objective splitting rules after [Answer]:).

[Answer]: B

### Question 4: Acceptance criteria format

Which testable format should stories use?

A) Numbered Given/When/Then scenarios with concrete inputs and observable outcomes where arithmetic or state matters; retain approved performance and agent-evaluation targets in a linked cross-cutting section.

B) Numbered precondition/action/expected-result tables with concrete inputs and observable outcomes; retain approved performance and agent-evaluation targets in a linked cross-cutting section.

X) Other (provide an equally verifiable format after [Answer]:).

[Answer]: A

## Review coverage

Business goal, success metrics, technical limits, user workflows, and scope are already established by the approved requirements and will be preserved, not re-asked. The four questions address persona representation, breakdown, granularity, format, acceptance structure, and journey organization. No implementation priorities, timelines, or sprint decisions are requested.

## Extension applicability

Security and Resiliency extensions remain disabled; approved explicit security and integrity requirements remain binding. PBT-01–PBT-10 implementation verification is N/A to story planning under the extension's stage applicability table. Full PBT and complementary example-based testing remain traceable quality obligations from NFR-08/AC-08; no tests or implementation are generated or claimed complete.

## Approval gates

Gate 1: Answer all questions, resolve ambiguities, and explicitly approve the finalized story plan before personas/stories generation. Gate 2: Explicitly approve generated stories and personas before Workflow Planning. Neither gate authorizes Construction; stop after the later Units Generation approval as requested.

### Artifact approval tracking
- [x] Receive explicit team approval of stories.md and personas.md before Workflow Planning.

## Inception closeout
As of 2026-11-20T19:58:21Z, all required Inception stage artifacts are explicitly approved, including final unit artifacts. This plan's tasks are complete; Construction has not started.
