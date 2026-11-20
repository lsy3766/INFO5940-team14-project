> **Historical input — superseded where later decisions differ.** Added 2026-11-20T20:05:59Z. The original text below is preserved unchanged as pre-Inception evidence; its pending-status statements and open questions describe that earlier snapshot, not the current project status. Inception is now complete. The [approved requirements](../../aidlc-docs/inception/requirements/requirements.md), [approved application design](../../aidlc-docs/inception/application-design/application-design.md) and [approved work unit](../../aidlc-docs/inception/application-design/unit-of-work.md) govern the final product/design baseline. See the [current collaboration plan](../COLLABORATION.md) for subsequent confirmed roles and meeting arrangements. No past meeting or approval is retroactively invented.

# Product Vision — Team Review Draft

Status: Pre-Inception input draft based on discussion with one team member. Not yet reviewed or approved by the full team.

## Product and users
Help people decide what to cook and eat through personalized five-day meal planning covering breakfast, lunch, and dinner. The audience is people who cook for themselves or others and need help deciding what to eat.

## Main problem
Users struggle to decide what to eat while accounting for preferences, available ingredients, and time. Plans also become outdated when users cook something different.

## Proposed MVP
- Record preferences, dietary exclusions, serving counts, and available cooking time. Do not model cookware constraints.
- Maintain a recipe library with ingredients, quantities, servings, instructions, and time estimates.
- Let users manually maintain ingredient inventory.
- Generate a personalized five-day meal plan covering all three daily meals using recipe search and inventory tools.
- Treat menus as suggestions: users can dynamically replace, skip, or adjust meals and serving counts. Planning alone must not deduct inventory.
- Aggregate required ingredients and generate a shopping list after subtracting inventory.
- Let users select a dish actually cooked from the recipe library or plan.
- Preview ingredient deductions and let users correct actual quantities. Deduct only after explicit confirmation.
- Check remaining meals against updated inventory and propose adjustments for confirmation.

## Agent behavior
The LLM selects tools to read inventory and preferences, retrieve recipes, validate a proposed plan, and revise it based on tool results. Deterministic tools calculate quantities and shortages. Persistent writes require user confirmation. The agent must report infeasible requests rather than claim success.

## Proposed boundaries for team review
- Plan five days of breakfast, lunch, and dinner with adjustable servings. Users customize the number of people; the agent proposes the number and combination of dishes accordingly, subject to user changes. Do not assume one dish per person.
- Intended recipe source: internet recipes, retrieved through an explicit search/retrieval tool and normalized into the recipe library with source links. An LLM connection alone is not internet access. Use source-attributed local recipe retrieval (RAG) as the reliable baseline; allow fetching recipes from user-provided public URLs. General live internet search remains conditional on a genuinely free, permitted provider and must not be assumed to come with the LLM. Missing information must be shown for confirmation rather than invented.
- No cookware matching, purchasing/payment integration, live supermarket pricing, photo recognition, or medical nutrition advice.
- No cloud deployment required; product runs locally.

## Open questions
- Which priority user scenarios and measurable benefits distinguish the product from asking a general chatbot?
- What practical upper limit for party size should the UI validate, and how should users specify appetite and shared dishes?
- Which internet recipe sources permit the intended use, and what cuisine coverage and initial collection size should be curated?
- Which units and substitutions are supported?
- What if recorded inventory is insufficient? How are duplicate confirmations and undo handled?
- How should takeout, leftovers, and partially eaten meals be recorded without double deduction?
- How much variety is required, and which constraints may be relaxed with user permission?
- Single local profile or multiple users?
- What measurable criteria define a useful plan and successful agent behavior?

No team approval or Inception completion is claimed by this draft.
