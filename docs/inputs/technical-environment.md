> **Historical input — superseded where later decisions differ.** Added 2026-11-20T20:05:59Z. The original text below is preserved unchanged as pre-Inception evidence; its pending-status statements and open questions describe that earlier snapshot, not the current project status. Inception is now complete. The [approved requirements](../../aidlc-docs/inception/requirements/requirements.md), [approved application design](../../aidlc-docs/inception/application-design/application-design.md) and [approved work unit](../../aidlc-docs/inception/application-design/unit-of-work.md) govern the final product/design baseline. See the [current collaboration plan](../COLLABORATION.md) for subsequent confirmed roles and meeting arrangements. No past meeting or approval is retroactively invented.

# Technical Environment — Pre-Inception Input

Status: Technical baseline selected with Shuyue's authorization for document preparation. Full-team review is pending. This is an input document, not an Application Design artifact. No application or live model connection is required for the milestone.

## Language, versions, package manager, framework
- Python 3.12.x, isolated with venv; pip for dependencies.
- FastAPI with Pydantic v2 for validated API requests and explicit agent tool schemas.
- Streamlit for a local browser UI: preferences, inventory, five-day/three-meal plan, recipe selection, and confirmation screens.
- SQLite through Python sqlite3 for recipes, inventory, plans, and cooking events. Use transactions for confirmations.
- HTTPX for bounded public recipe-page retrieval; Beautiful Soup 4 and JSON-LD Recipe parsing for supported pages.
- pytest and FastAPI TestClient for tests. Choose compatible stable dependency releases and pin exact versions at Construction start; no dependency installation now.
- Direct provider SDK and a small explicit tool loop; no mandatory agent orchestration framework or vector database.

## Cloud provider and deployment model
No cloud hosting provider. UI and backend run on team laptops, bound to localhost. Planned commands after implementation: `python -m uvicorn app.api:app --host 127.0.0.1 --port 8000` and `python -m streamlit run app/ui.py`. Paths are planned conventions, not existing files. External internet access is needed only for the hosted model and optional page retrieval. SQLite data remains local.

## LLM provider and model
Planned provider: Google Gemini Developer API. Planned model: `gemini-3.5-flash-lite`, using the free tier only and native function calling. Official pricing and model documentation were checked on 2026-11-17; actual account access and quotas remain untested. Recheck at Construction start. Stop and report quota/access errors; never enable billing, silently choose a paid model, or retry indefinitely. Local Ollama with a tool-capable open model is a possible future alternative, subject to hardware testing, not an already available fallback.

Development assistant: OpenAI Codex, with official AI-DLC v1.0.1 rules installed. Product model access is separate from the coding assistant.

## Recipe retrieval and RAG
The model's internal knowledge is not a maintained recipe database. Store source-attributed recipes in SQLite with ingredients, quantities, base servings, meal tags, preparation time, and retrieval date. Retrieve candidates using ingredient/tag filters and keyword ranking; provide selected source text to the model before generation. This is retrieval-augmented generation (RAG); embeddings are not required for the baseline.

Internet-first preference is supported by an explicit fetch tool for user-provided public recipe URLs and supported sources. Parse JSON-LD Recipe metadata when available, validate it, and save reusable records. Blocked, unsupported, or incomplete pages return clear errors and allow pasted recipe text; label user-provided content appropriately. Local RAG is the dependable baseline for planning and offline recipe access. General internet search is an optional extension requiring a separately verified zero-cost provider. Do not rely on Gemini's built-in Google Search grounding: the selected model's standard free tier does not list it as available. Never promise a universal free web-search service.

Preserve source links, respect source terms and licensing, and curate suitable seed recipes for demos. If retrieval yields too few suitable recipes, explain the shortage or ask to relax a condition. Model-suggested new recipes must be labelled unverified, reviewed before inventory use, and never assigned fabricated sources.

## Product constraints
Five days covering breakfast, lunch, and dinner; custom positive integer party size; meal-specific overrides. The agent proposes dishes and quantities based on party size, preferences, and time, and users can change them. No one-dish-per-person rule. Plans are suggestions and can be changed or skipped; planning never deducts stock. Preview actual consumption, allow quantity edits, then require explicit confirmation. Prevent duplicate deductions and negative stock; support undo. Record takeout and leftovers separately to avoid deducting raw ingredients twice. No cookware matching.

## Test framework and verification
pytest unit tests cover serving arithmetic, compatible unit handling, shopping aggregation, and exclusion filters. Integration tests cover a complete five-day/three-meal plan, plan modification, preview without mutation, confirmation exactly once, stale inventory, insufficient quantities, undo, recipe retrieval failure, and model quota failure. Use deterministic fake model/tool responses for most tests. Live tool-calling evaluation happens during Construction, not now; evaluate both final plan validity and actual tool use.

## Prohibited libraries and alternatives
| Library or approach | Reason | Alternative |
| --- | --- | --- |
| pickle for imported or user-provided data | Untrusted deserialization can execute code | JSON validated with Pydantic |
| PyYAML yaml.load with unsafe loaders | Unnecessary object construction from untrusted data | JSON; safe_load only if YAML is actually needed |
| Model-generated code executed via eval/exec | Unbounded execution is unnecessary for cooking tools | Explicit allowlisted Python functions |
| Paid search or model dependencies | Violates the zero-spend requirement | Local RAG and free-tier model access; report unavailability |

## Security basics
Keep keys only in ignored local environment files and server-side; do not commit keys or include them in audit logs. Use fictional demo profiles because hosted free-tier data may be used for provider product improvement. Validate tool arguments and reject invalid servings or incompatible units. Treat web text as untrusted data, never agent instructions. Fetch only public HTTP(S) pages, reject private/loopback destinations and unsafe redirects, bound download size and time, and avoid arbitrary URL execution. Validate exclusions before recommending recipes; do not guarantee allergy safety or infer food spoilage. Inventory confirmation is enforced in backend logic, not by model promises; use transactions, stock-version checks, and idempotency identifiers. Run locally without public network exposure.

## Typical endpoint example (illustrative; not implemented)
POST /api/cooking-events/preview
Request: {"recipe_id": "recipe-001", "servings": 2}
Response: {"preview_id": "preview-001", "inventory_version": 7, "deductions": [{"ingredient_id": "egg", "quantity": 2, "unit": "count"}], "shortages": []}
Preview changes no stock. A separate confirmation request supplies preview_id and a unique request identifier; revalidate stock and commit once. Return a validation error for invalid servings, or a conflict for stale/insufficient stock.

## Typical function example (illustrative; not implemented)
```python
from decimal import Decimal

def required_quantity(base_quantity: Decimal, base_servings: int, target_servings: int) -> Decimal:
    if not base_quantity.is_finite() or base_quantity < 0:
        raise ValueError("Invalid quantity")
    if base_servings <= 0 or target_servings <= 0:
        raise ValueError("Invalid servings")
    return base_quantity * Decimal(target_servings) / Decimal(base_servings)
```

## Typical test example (illustrative; not implemented)
```python
from decimal import Decimal
import pytest
from app.services.quantities import required_quantity

def test_scales_ingredient_and_rejects_invalid_servings():
    assert required_quantity(Decimal("150"), 1, 2) == Decimal("300")
    with pytest.raises(ValueError):
        required_quantity(Decimal("150"), 1, 0)
```
Module paths above are proposed conventions for later implementation. No application code is generated by this document.

## References
- https://fastapi.tiangolo.com/
- https://docs.streamlit.io/get-started/installation/command-line
- https://ai.google.dev/gemini-api/docs/models/gemini-3.5-flash-lite
- https://ai.google.dev/gemini-api/docs/function-calling
- https://ai.google.dev/gemini-api/docs/pricing/
