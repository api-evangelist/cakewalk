---
name: Review and approve or decline an access task
description: Find the tasks awaiting an approver, inspect a task, and approve or decline it.
api: https://open-api.getcakewalk.io/api/v1
operations:
  - GET /Tasks?userId={userId}
  - GET /Tasks/{taskId}
  - POST /Tasks/{taskId}/Approve
  - POST /Tasks/{taskId}/Decline
generated: '2026-07-18'
method: generated
source: https://www.cakewalk.security/docs/open-api-and-mcp/api-reference
---

# Review and approve or decline an access task

In Cakewalk, a Task is the action an approver must take to complete an access Request. Use this
skill to clear an approver's queue.

## Authentication
Send `X-API-KEY` and `X-API-SECRET` headers; the key must be read-and-write to approve/decline.
Base URL: `https://open-api.getcakewalk.io/api/v1`.

## Steps
1. **List the approver's tasks.** `GET /Tasks?userId={userId}` for the approver whose queue you
   are clearing (paginate with `links.next`).
2. **Inspect a task.** `GET /Tasks/{taskId}` to read the request context before deciding.
3. **Decide.**
   - Approve: `POST /Tasks/{taskId}/Approve`
   - Decline: `POST /Tasks/{taskId}/Decline`

## Rules
- Errors are RFC 7807 ProblemDetails; handle `401`, `403` (not this approver / read-only key),
  `404` (task not found), and `409` (task already actioned).
- Respect the 1,000 requests-per-minute rate limit; back off on `429`.
- Approve/Decline are not idempotent — re-fetch the task with `GET /Tasks/{taskId}` before retrying.
- A completed actionable task fires the `action_task.actionable` webhook
  (see asyncapi/cakewalk-webhooks.yml) — verify the `X-SIGNATURE` on any inbound webhook.
