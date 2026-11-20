# Component Methods and Contracts

Status: Explicitly approved by the team, recorded at 2026-11-20T19:42:32Z. Documentation only; no implementation or external connection performed. Unit artifacts require separate approval.

## Contract conventions

Signatures below are design notation, not executable application code. Inputs/outputs are named domain contracts. Successful methods return the named typed result; documented typed exceptions propagate to one central C01 API mapper or C08 tool mapper. Model outputs/tool arguments remain untrusted and require validation.

| Type | High-level fields and meaning |
| --- | --- |
| Quantity | Finite nonnegative decimal amount and explicit unit: g/kg, ml/l or count; no density/count-weight assumptions. |
| Preferences / MealOverride | Exclusions, preferences, diner count integer 1–10, 15/45/45-minute defaults and per-meal overrides; no per-person appetite model. |
| RecipeRecord / RecipeCandidate | Stable ID, title, ingredient quantities/units, base servings, instructions, time estimate, meal/cuisine tags, provenance, retrieval date, completeness/review/eligibility status. |
| RecipeQuery / CoverageReport | Keyword/ingredient/tag criteria; count of eligible complete distinct recipes and coverage/shortfall information. |
| InventorySnapshot | Ingredient quantities and inventory version; read result, not a stock reservation. |
| PlanDraft / ValidatedPlan | Five-day 15-slot proposal, dishes, scaled quantities, constraint decisions/permissions, recipe references and validation report; may expose shortages. |
| ShoppingList | Aggregate compatible requirements, inventory credited once, nonnegative shortages and incompatible-unit findings. |
| ProposalRef / ConfirmationContext | Server-recognized proposal identity/revision and payload binding; explicit user-action origin, action type and scope. Model-supplied booleans cannot manufacture confirmation. No login/auth feature implied. |
| ConsumptionPreview | Preview identity, selected recipe revision, servings/actual quantities, expected inventory version, deductions/shortages, prospective stock and validation results; preview is not a committed event. |
| CookingResult / UndoResult | Recorded event/compensation identity, resulting stock/version and request identifier; replay returns the recorded outcome. |
| ErrorDetails | Stable category, safe user message, relevant field/conflict/shortage details, recovery action and trace correlation; no API key or hidden prompt leakage. |
| PlanningTrace | Redacted tool names, validated arguments/results, deterministic validation and revision/failure steps; diagnostic data, never write authorization. |

## Interface inventory

| Owner | Signature | Purpose / contract boundary |
| --- | --- | --- |
| C01 | handle(request: ValidatedRequest, user_action: UserActionContext) -> ApiResponse | Validate request and derive action-specific confirmation context; map typed exceptions centrally; final result only. |
| C01 | render(result: ApiResponse) -> ViewState | Show editable previews, errors and waiting/final states in English; no DB or SDK access. |
| C02 | read_preferences() -> Preferences | Return single local profile. |
| C02 | save_preferences(change: PreferencesChange, intent: ManualSave) -> Preferences | Validate diner/time fields and explicit save; C10 persists. |
| C03 | search(query: RecipeQuery) -> RecipeCandidates | Local filters/ranking; candidates passed to automatic planning must be eligible. |
| C03 | get_recipe(id: RecipeId) -> RecipeRecord | Return record and eligibility/provenance metadata; not-found is explicit. |
| C03 | assess_coverage() -> CoverageReport | Count at least 45 distinct eligible complete recipes, Chinese/Western and all meal types; report deficits, not invented content. |
| C03 | preview_import(source: PublicUrlOrPastedText) -> RecipeImportPreview | Use C09 fetch/parser for URLs; label pasted text; return missing/eligibility findings without saving. |
| C03 | save_reviewed_recipe(preview: ProposalRef, corrections: RecipeCorrections, consent: ConfirmationContext) -> RecipeRecord | Validate completeness, provenance/use eligibility and confirmation; save via C10. Incomplete/unreviewed records cannot enter automatic use. |
| C03 | preview_variant(base: RecipeId, change: Substitution) -> RecipeVariantPreview | Preserve source lineage; no implicit conversion or approval. |
| C03 | approve_variant(preview: ProposalRef, consent: ConfirmationContext) -> RecipeRecord | Revalidate full variant and exclusions before eligible use; persist only on explicit approval. |
| C04 | snapshot() -> InventorySnapshot | Read stock with version, no mutation. |
| C04 | edit_stock(change: StockEdit, intent: ManualSave) -> InventorySnapshot | Validate nonnegative finite quantities and explicit manual action; persist/version via C10. |
| C05 | preview_change(plan: PlanRef, change: MealChange) -> PlanDraft | Replace/skip/adjust servings or annotate; no auto-save/deduction. |
| C05 | save_plan(draft: ProposalRef, consent: SaveIntent) -> SavedPlan | SaveIntent is direct manual save or explicit confirmation bound to an agent proposal; revalidate before persistence. |
| C05 | shopping(plan: PlanDraftOrSavedRef, stock: InventorySnapshot) -> ShoppingList | C07 aggregates and credits stock once; read-only. |
| C05 | save_annotation(plan: PlanRef, annotation: TakeoutOrLeftovers, intent: ManualSave) -> SavedPlan | Annotation only; no leftover portion account or raw ingredient deduction. |
| C06 | preview_consumption(selection: RecipeSelection, servings: ServingCount, actual: QuantityEdits) -> ConsumptionPreview | Check eligible recipe/units; use current stock; no stock/event commit. |
| C06 | confirm(preview: PreviewRef, consent: ConfirmationContext, request_id: RequestId) -> CookingResult | Validate explicit user action; call C10 atomic operation; no model path. |
| C06 | preview_undo(event: CookingEventId) -> UndoPreview | Read history/current stock and explain proposed compensation; no mutation. |
| C06 | confirm_undo(preview: ProposalRef, consent: ConfirmationContext, request_id: RequestId) -> UndoResult | Explicit safe once-only compensation via C10; preserve original history. |
| C07 | scale(recipe: EligibleRecipe, servings: ServingCount) -> IngredientRequirements | Deterministic arithmetic; approved units only. |
| C07 | aggregate(requirements: IngredientRequirements, stock: InventorySnapshot) -> ShoppingList | Sum compatible units, subtract stock once, floor shortages at zero. |
| C07 | validate_plan(draft: PlanDraft, profile: Preferences, permissions: RelaxationPermissions) -> PlanValidation | Exclusions hard; time/repetition exceptions only with explicit bounded permission; validate recipe eligibility and meal structure. |
| C07 | validate_consumption(preview: ConsumptionPreview, current: InventorySnapshot) -> ConsumptionValidation | Current version, sufficient nonnegative quantities and recipe integrity; authoritative validation participates in C10 transaction. |
| C07 | validate_undo(event: CookingEvent, current: InventorySnapshot, history: EventHistory) -> UndoValidation | Define safe/once-only high-level checks; detailed safe-undo policy/algorithm is a future Functional Design task. |
| C08 | propose(request: MealPlanningRequest) -> PlanningProposal | Bounded tool loop using C02–C05/C07/C09; validated draft plus trace, no persistent domain writes. |
| C08 | revise_remaining(plan: PlanRef, inventory_version: Version) -> PlanningProposal | Read latest saved plan/stock, detect impacts and propose changes; saving remains separate C05 action. |
| C08 | dispatch_tool(call: ModelToolCall) -> ToolResult | Allowlist and typed argument validation; only exposed read/compute tools, no commits. |
| C09 provider | next_turn(context: RetrievedContext, tools: ToolSchemas, deadline: Deadline) -> ModelTurn | Native function-call/text response; bounded free-tier provider call only after future access verification. |
| C09 recipe | fetch_parse(url: PublicUrl, limits: FetchLimits) -> UntrustedRecipeDocument | Validate public destination/redirects, size/time and supported source; parse JSON-LD when available, no arbitrary execution. |
| C10 | read_profile() -> Preferences; read_stock() -> InventorySnapshot; read_plan(ref: PlanRef) -> SavedPlan; read_recipe(id: RecipeId) -> RecipeRecord | Typed persistence reads; add catalog/history reads as required by C03/C06 without exposing SQL. |
| C10 | save_profile(command: ValidatedProfileWrite) -> Preferences; save_recipe(command: ValidatedRecipeWrite) -> RecipeRecord; save_plan(command: ValidatedPlanWrite) -> SavedPlan | Only application services pass backend-authorized validated commands; raw model arguments are not valid commands. |
| C10 | apply_stock_edit(command: ValidatedStockEdit) -> InventorySnapshot | Manual edit changes stock/version consistently; cannot forge cooking history. |
| C10 | store_preview(preview: ValidatedPreview) -> PreviewRef; read_preview(ref: PreviewRef) -> ConsumptionPreview | Technical preview-state storage never commits cooking or stock; binds later confirmation to reviewed content. |
| C10 | atomic_confirm(command: AuthorizedCookingCommand) -> CookingResult | Recheck bound preview/current authoritative data inside transaction; couple stock, event, version and idempotency outcome; all-or-nothing. |
| C10 | atomic_undo(command: AuthorizedUndoCommand) -> UndoResult | Recheck original event, current state and already-undone status within transaction; atomically append compensation and record outcome. |
| C10 | lookup_outcome(request_id: RequestId) -> RecordedOutcome | Recover uncertain final responses and replay same confirmed command without duplicate effects. |

## Typed failure semantics

| Category | Behavior |
| --- | --- |
| ValidationError / NotFoundError | Reject invalid input/missing reference; explain field or resource; no partial writes. |
| UnsupportedSourceError / RecipeEligibilityError | Reject unsupported/unsafe URLs or ineligible/incomplete records; offer permitted source-text correction when appropriate. |
| InfeasiblePlanError | Explain validated constraints/candidate shortage; may suggest user-approved soft relaxation, never exclusion bypass. |
| StaleStockError / InsufficientStockError | Reject cooking commit, show latest mismatch/shortage and require correction plus a fresh preview. |
| ConfirmationRequiredError / ConflictingRequestError | Reject absent/mismatched explicit intent or request-ID reuse for a different payload/action. Successful identical replay returns original typed success. |
| UnsafeUndoError / AlreadyUndoneError | No second compensation or history deletion; return safe recorded outcome for identical successful request replay. |
| ProviderUnavailableError / ToolFailureError / TimeoutError | Report failure with bounded user-controlled recovery; no false success, paid fallback or indefinite retry. |

HTTP status values, exact schemas, preview lifetime, duplicate-key storage, decimal precision, undo conflict rules and loop/fetch caps are deferred detailed-design decisions. Categories and invariants are fixed here; all numeric caps must preserve the existing 60-second model limit and approved usability targets. No new user/product decisions are silently substituted for approved requirements.
