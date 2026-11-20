# Unit and Module Dependencies

Status: Explicitly approved by the team, recorded at 2026-11-20T19:58:21Z. Units Generation and Inception are complete. STOP before Construction. Documentation only; no application or test files created.

## Work-unit dependency matrix

| Unit | Other team-owned units required | External/runtime dependencies | Release interpretation |
| --- | --- | --- | --- |
| UOW-01 | None; no inter-unit edges | Local Python UI/API runtime, local SQLite, conditionally verified free-tier Gemini API and permitted supported recipe sources | One whole-application unit; internal modules are not independent services |

Local recipe retrieval works after an eligible collection exists; hosted model generation requires network access. Public source/model dependencies have no presumed free availability or approved source list. No general search, paid fallback or additional deployment is introduced.

## Internal allowed-call matrix

Rows are callers; columns are targets. X means an allowed component-level call, subject to method-level restrictions; blank means no direct call. It is an internal module matrix, not inter-service networking. Shared contract type imports are omitted from runtime edges and are one-way toward the authoritative catalog.

| Caller | M01 | M02 | M03 | M04 | M05 | M06 | M07 | M08 | M09 | M10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| M01 |  |  |  |  |  |  | X |  |  | X |
| M02 |  |  |  |  |  |  | X |  | X | X |
| M03 |  |  |  |  |  |  | X |  |  | X |
| M04 | X | X | X |  |  |  | X |  |  | X |
| M05 |  | X | X |  |  |  | X |  |  | X |
| M06 | X | X | X | X | X |  |  | X |  |  |
| M07 |  |  |  |  |  |  |  |  |  |  |
| M08 | X | X | X | X |  |  | X |  | X |  |
| M09 |  |  |  |  |  |  |  |  |  |  |
| M10 |  |  |  |  |  |  | X |  |  |  |

This translates the approved C01–C10 matrix, with M06=C01, M01=C02, M02=C03, M03=C04, M04=C05, M05=C06, M07=C07, M08=C08, M09=C09 and M10=C10. M08 calls read-only subsets of capabilities; an X is never permission to call their save/commit methods. M06 cannot directly access M09 or M10. M10 calls pure M07 but never imports M05/M08 orchestration, avoiding a dependency cycle. SQLite is reachable only through M10.

## Shared resource and contract ownership

| Shared item | Authoritative owner | Consumers / restriction |
| --- | --- | --- |
| Typed contracts | One catalog, design basis component-methods.md; future app/contracts/ | All modules; capability owners/consumers review changes, no duplicate incompatible definitions |
| Recipe eligibility/provenance | M02 semantic ownership; M10 storage | M04/M05/M08 use complete reviewed eligible records; M09 fetch results confer no authority |
| Stock/version | M03 read/manual-edit responsibility; M10 physical persistence | M04/M05/M08 use snapshots; only M10 mutates stock |
| Plan drafts/saved plans and annotations | M04 semantic ownership; M10 storage | M08 reads/proposes, M06 confirms; no implicit deduction |
| Cooking previews/events/undo outcomes | M05 workflow ownership; M10 atomic store | Preview non-consuming; confirmation is bound; original events retained |
| Request outcome/idempotency | M10 | M05 retrieves/replays identical authorized request; different payload/action conflicts |
| Keys and external access | Server-side M09 configuration | Never UI/model-tool writable, never committed/logged; no automatic paid switch |
| Diagnostic traces | M08, redacted presentation at M06 | Future evaluation only, no user consent or write authority |

## Integration checkpoints — documentation, not tests executed

1. Recipe boundary: M09 untrusted data -> M02 eligibility/review -> M10 confirmed persistence. Review required fields, permitted use, correction/cancellation and exclusion from automatic use when incomplete.
2. Planning boundary: M06 request -> M08 eligible retrieval/profile/stock reads -> M07 deterministic validation -> M04 confirmed save. Verify 15 slots, 1–10 diners, time/repetition permission, source references and zero stock writes.
3. Shopping boundary: M04 selected requirements + M03 snapshot -> M07 totals/shortages. Credit stock once; reject incompatible units; preserve zero-mutation behavior.
4. Transaction boundary: M06 explicit intent -> M05 command -> M10 authoritative revalidation/atomic commit. Preserve stock/event/version/idempotency all-or-nothing behavior, stale/shortage rejection, same-ID replay, conflict handling and safe once-only compensation.
5. Remaining meals/annotations: post-change snapshot -> M08 proposal -> M04 separate confirmation. Takeout and leftovers remain non-consuming annotations.
6. Failure/quality boundary: M09 quota/retrieval failures -> M08/M06 typed messages and bounded retry, diagnostic trace/evaluation, no false success, paid fallback or unnoticed write.

Contract changes require capability and affected-consumer review, including relevant role leads; document the changed request/result, error/confirmation implications and downstream scenario coverage before future implementation. API mapping and tool error mapping stay consistent with typed-success/typed-error choice.

## Future dependency order, not authorized execution

For later implementation planning, establish shared contract and pure policy definitions before dependent capability logic; gateway interfaces/atomic operations must be available for persistent workflows. Recipe/inventory/profile capabilities provide data for planning and cooking. Planning orchestration depends on those contracts, deterministic validation and provider adapter; UI integrates capability and planning results. Mock adapters can support deterministic checks before live feasibility tests. This is a partial dependency order, not a sprint plan or permission to begin Construction; exact sequencing belongs to later approved stages.

Dependency validation: ten uniquely mapped modules, no internal directed cycle in the allowed-call graph, one whole-application unit, no cross-service transaction split. No source/test files, builds or model calls were performed to validate this documentation.
