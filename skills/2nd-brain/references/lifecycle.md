# Knowledge status lifecycle

Status values are exactly:
- `draft`: not yet confirmed by the developer. Default for a new document.
- `verified`: developer explicitly confirmed the content. Record `verified_at` only if the date is known.
- `outdated`: may no longer match implementation or current decisions. Use with warnings.
- `deprecated`: no longer current; keep as history and link `superseded_by` when known.

| Transition | Required rule |
|---|---|
| New → draft | Normal after content approval. |
| draft → verified | **Explicit developer verification**; never automatic. |
| verified → outdated | Explain observed conflict and propose change; obtain write approval. |
| outdated → verified | Correct content, then obtain fresh developer verification. |
| draft / verified / outdated → deprecated | Developer approves deprecation; keep replacement link if it exists. |
| deprecated → active status | Ask developer; clarify whether to restore or create a new canonical document. |

Status modifications are file writes and must be included in the approval plan. Even a clearly stale document must not be silently marked `outdated`.

Reading policy:
- `verified` does not excuse checking current code when relevant.
- `draft` and `outdated` may be used for investigation, but never described as confirmed current facts.
- `deprecated` is background/history, not current instruction.

No automatic expiry or review date thresholds are fixed in v1.0. Propose review when there is concrete evidence of change or an explicit review request. Do not invent a verification person or timestamp.
