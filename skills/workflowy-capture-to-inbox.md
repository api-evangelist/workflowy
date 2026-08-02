---
name: Capture notes and tasks to the Workflowy Inbox
description: Quickly capture text, tasks, and structured outlines into a user's
  Workflowy Inbox (or any target) via the Workflowy API.
api: openapi/workflowy-api-openapi.yml
operations: [createNode, listTargets]
generated: '2026-07-21'
method: generated
source: https://workflowy.com/api-reference/
---

# Capture notes and tasks to the Workflowy Inbox

Authenticate every call with `Authorization: Bearer <API_KEY>` (keys come from
https://workflowy.com/api-key/). Base URL: `https://workflowy.com/api/v1`.

## Steps

1. (Optional) `listTargets` — `GET /targets` returns the user's system targets
   (`inbox`, `today`, ...) and custom shortcuts. Use a shortcut `key` as a
   `parent_id` when the user names a destination ("my reading list").
2. `createNode` — `POST /nodes` with `{"parent_id": "inbox", "name": "...",
   "position": "top"}`. `parent_id` accepts `"inbox"`, `"None"` (root), a full
   or 12-character node id, a node URL, a shortcut key, or calendar targets
   (`"today"`, `"tomorrow"`, `"2026-01-15"`, ...). Calendar nodes are created
   on demand.

## Rules

- Markdown in `name` is parsed: `**bold**`, `*italic*`, `` `code` ``,
  `[text](url)`, `[YYYY-MM-DD]` dates; prefixes set layout — `#`/`##`/`###`
  headers, `- [ ]` todo, `- [x]` completed todo, code fences, `>` quotes.
- Multiline `name`: the first line becomes the parent; `\n\n` creates separate
  child nodes, a single `\n` is joined into a space. Use this to capture whole
  outlines in one call.
- Put extended detail in `note`, not in `name`.
- The response is `{"item_id": "<uuid>"}` — keep it if you need to update or
  move the node later. There is no idempotency key: do not blindly retry a
  create that may have succeeded, or you will duplicate nodes.
