---
name: Plan and work today's tasks in Workflowy
description: Read, add, complete, and reorganize items on the user's Workflowy
  calendar day nodes to run a daily-planning loop.
api: openapi/workflowy-api-openapi.yml
operations: [listNodes, createNode, completeNode, uncompleteNode, moveNode, updateNode]
generated: '2026-07-21'
method: generated
source: https://workflowy.com/api-reference/
---

# Plan and work today's tasks in Workflowy

Authenticate with `Authorization: Bearer <API_KEY>`. Base URL:
`https://workflowy.com/api/v1`.

## Steps

1. `listNodes` — `GET /nodes?parent_id=today` lists today's items. **Calendar
   keys return 404 if the day node hasn't been created yet** — that means the
   day is empty, not an error; materialize it with a create or move.
2. `createNode` — `POST /nodes` with `parent_id: "today"` (or `"tomorrow"`,
   `"next_week"`, `"YYYY-MM-DD"`) and `name: "- [ ] task"` to add a todo.
3. `completeNode` / `uncompleteNode` — `POST /nodes/{id}/complete` and
   `POST /nodes/{id}/uncomplete` toggle completion (tracked via `completedAt`).
4. `moveNode` — `POST /nodes/{id}/move` with `parent_id: "tomorrow"` to defer
   an item; calendar destinations are created on demand.
5. `updateNode` — `POST /nodes/{id}` changes `name`/`note`/`layoutMode`; only
   the parameters you pass are changed.

## Rules

- Lists come back **unordered** — always sort by `priority` (lower first)
  before presenting.
- Timestamps (`createdAt`, `modifiedAt`, `completedAt`) are Unix seconds.
- Mutations return `{"status":"ok"}`; treat anything else as failure.
- No idempotency keys — confirm state with `listNodes`/`getNode` before
  retrying writes.
