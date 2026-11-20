# Authentic Chat Transcript Supplement — Preparation Protocol

Status: Instructions only. No original chat transcript has been exported/imported or AI reply reconstructed. Use the 20-entry inventory in submission-readiness-review.md as the reconciliation target, not as source text.

## Source

Use this same Codex chat from the actual workflow-start request through the final collaboration confirmation, preserving the order of user and assistant messages. The pre-workflow read-only rule inspection was explicitly outside the audit start. Later submission-repair chats are additional interactions, not part of the original 20 gaps, and should also be retained.

Official CLI documentation describes `/export` for a complete Markdown conversation export: https://learn.chatgpt.com/docs/developer-commands?surface=cli . This is a CLI feature, not a verified desktop menu instruction. Use it only if the same saved session is genuinely accessible in the CLI and that command is available; confirm the exported thread identity and completeness. Do not create a new conversation and treat it as the original export.

No current official desktop-specific export path was established in this check. In the desktop app, use an actual available full-conversation export/copy facility if present, or copy the visible original user/assistant messages in order. Do not invent an export menu. Screenshots may support provenance but searchable verbatim text is preferable. The available read_thread tool advertises recent status/turn summaries; those summaries are not a substitute for original transcript text. A ChatGPT account export should not be assumed to include this Codex chat.

## Accepted formats and provenance

Prefer UTF-8 Markdown or plain text containing message boundaries, speaker (user/assistant), original text/links/code blocks and original order. JSON/JSONL is also acceptable if it contains actual message content and role/message identifiers, not summaries. Include assistant progress/commentary and final responses where the source records them. Include actual tool/command records only if exported and relevant to the course audit requirements; never hidden reasoning or credentials.

Retain the raw export unchanged privately until reviewed for secrets. Record chat/session identity where available, source surface, export method, import time and original message IDs/sequence. Keep original timestamps only when supplied by the source; mark absent timestamps as “not available”. Never use audit record times as fabricated message-send times. Keep a checksum of the actual exported file when available; no checksum is claimed now. Label any separately redacted public copy and retain a private original.

## Mapping to the 20 missing ranges

The inventory's line numbers identify existing audit entries, not original chat timestamps. For each of the 20 rows:

1. Match the audit entry title and exact user-input text to the original chat turn. The welcome/initial acknowledgement rows may map to multiple assistant messages within the same opening turn; do not invent 20 distinct turns or duplicate identical text.
2. Collect all actual assistant commentary and final messages associated with that interaction, retaining original order and citations. Reconcile full approval wording, not merely the stored Full Approval Prompt block.
3. Record a mapping row: audit entry title and original line reference; export message IDs or ordinal range; source file/section; exact or partial coverage; missing timestamp/content notes. Never guess a match solely from similar stage summaries.
4. Append a clearly titled transcript supplement to the audit or an auditable linked supplement file, with import/recording timestamp distinct from original message time. Preserve original historical audit entries, questions and approvals.
5. Mark each inventory range complete only when actual original text is available and matched. Report missing/partial ranges explicitly. Existing real approvals are not re-created or re-timestamped.

The 20-row inventory remains the authoritative to-do list until genuine source material is supplied and reconciled. This protocol does not resolve those gaps by itself.
