# Content gap summary array configuration

## Type
Execution example

## Purpose
Examples and validation rules for `content_gap_summary_json_array`.

## When to use
Read this when recording or reviewing resolved or unresolved content gaps for a visit or stop.

## Knowledge base

This document is part of the Autoura configuration split proposal generated from the live Autoura docs.

Back to main visit document: https://www.autoura.com/core/pai/docs/visit_config.md

Back to main stop document: https://www.autoura.com/core/pai/docs/stop_config.md

---

## Content gap summaries

Content gap summaries record what visitor-facing information was missing, incorrect, outdated, or unclear, and what was added or changed.

Use this array field:

`content_gap_summary_json_array`

PAI update and internal storage use `content_gap_summary_json_array`. Consumer-facing provenance MCP responses and public Visit Get/Search responses expose the decoded list as `content_gaps`.

Public Visit Search supports `order_by=content_gaps`. This returns Visits with the most recently changed content gaps first, using the platform-maintained internal content-gap timestamp. That timestamp is not included in the public response, and no separate content-gap date or fixed-status field is added.

Public Visit Search also supports `has_content_gaps=true|false`. This filters only on whether the Visit has one or more `content_gaps` items. It does not filter on item status or resolution.

Example:

```json
[
  {
    "status": "resolved",
    "type": "missing_information",
    "title": "Step-free entrance details",
    "note": "The visit did not explain which entrance is step-free. The accessibility guidance now identifies the step-free entrance and where to find it.",
    "date_created": "2026-07-28",
    "date_resolved": "2026-07-30"
  }
]
```

Fields:

| Field | Meaning |
|---|---|
| `status` | `unresolved` or `resolved` |
| `type` | `missing_information`, `incorrect_information`, `outdated_information`, or `clarification` |
| `title` | Short public-display-safe title for the content gap |
| `note` | Required public-display-safe explanation of both the problem and what was added or changed |
| `date_created` | Date the content gap was recorded, in `YYYY-MM-DD` format |
| `date_resolved` | Resolution date in `YYYY-MM-DD` format. Use an empty string while unresolved |

Every row must be an object and must contain `status`, `type`, `title`, `note`, `date_created`, and `date_resolved`.

Unknown fields are rejected.

For `resolved` rows, `date_resolved` is required. For `unresolved` rows, `date_resolved` must be an empty string.

The `note` must be safe for public display. It must describe both sides of the change:

- what information was missing, incorrect, outdated, or unclear
- what information was added or changed

Do not include internal task instructions, private discussion, unsupported claims, or source material that should instead be recorded as a Fact.

---
