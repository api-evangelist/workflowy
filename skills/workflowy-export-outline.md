---
name: Export and rebuild a full Workflowy outline
description: Export every node in a Workflowy account as a flat list and
  reconstruct the outline tree for analysis, backup, or migration.
api: openapi/workflowy-api-openapi.yml
operations: [exportNodes, getNode]
generated: '2026-07-21'
method: generated
source: https://workflowy.com/api-reference/
---

# Export and rebuild a full Workflowy outline

Authenticate with `Authorization: Bearer <API_KEY>`. Base URL:
`https://workflowy.com/api/v1`.

## Steps

1. `exportNodes` — `GET /nodes-export` returns every node the user owns as a
   flat list. **Rate limit: 1 request per minute** (large responses) — never
   poll this endpoint; cache the result and retry no sooner than 60s after a
   failure.
2. Rebuild the tree client-side: group by `parent_id` (`null` = top level) and
   sort each sibling group by `priority` ascending.
3. For a single subtree instead of the whole account, prefer `getNode` +
   `listNodes` walks over repeated exports.

## Rules

- Export rows include a `completed` boolean in addition to `completedAt`.
- `name` may contain inline HTML (`<b>`, `<i>`, `<s>`, `<code>`, `<a href>`)
  and `data.layoutMode` distinguishes bullets, todos, headers, code and quote
  blocks — preserve both when migrating.
- The export is a point-in-time snapshot; re-export (respecting the rate
  limit) rather than diffing stale data.
