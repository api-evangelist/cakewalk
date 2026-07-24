---
name: Remove a user's access to an app
description: Create an access request that revokes a specific user's access to a Cakewalk work app.
api: https://open-api.getcakewalk.io/api/v1
operations:
  - POST /Requests/RemoveAccess
  - GET /WorkApps/{workAppId}/Accesses
  - GET /Requests
generated: '2026-07-18'
method: generated
source: https://www.cakewalk.security/docs/open-api-and-mcp/api-reference
---

# Remove a user's access to an app

Use this skill to revoke a user's access to a Cakewalk work app by creating a remove-access request.

## Authentication
Send `X-API-KEY` and `X-API-SECRET` headers on every request; the key must be read-and-write.
Base URL: `https://open-api.getcakewalk.io/api/v1`.

## Steps
1. **Confirm the existing access.** `GET /WorkApps/{workAppId}/Accesses` to verify the target
   user currently has access to the app (paginate with `links.next`).
2. **Create the remove request.** `POST /Requests/RemoveAccess` identifying the target user and
   work app, with a purpose describing why access is being removed. On success you receive a
   `requestId`.
3. **Track it.** `GET /Requests` to confirm the request and follow it through the approval policy.

## Rules
- Errors are RFC 7807 ProblemDetails; handle `400`, `401`, `403`, `404` (access/app not found),
  and `409`.
- Respect the 1,000 requests-per-minute rate limit; back off on `429`.
- Not idempotent — verify with `GET /Requests` before retrying a submission.
