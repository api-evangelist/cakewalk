---
name: Grant a user access to an app
description: Create an access request that grants a specific user access to a Cakewalk work app, optionally at a chosen permission level.
api: https://open-api.getcakewalk.io/api/v1
operations:
  - POST /Requests/GrantAccess
  - GET /WorkApps
  - GET /WorkApps/{workAppId}/PermissionLevels
  - GET /Requests
generated: '2026-07-18'
method: generated
source: https://www.cakewalk.security/docs/open-api-and-mcp/api-reference
---

# Grant a user access to an app

Use this skill to grant a user access to a Cakewalk work app by creating an access request.

## Authentication
Send both `X-API-KEY` and `X-API-SECRET` headers on every request. The key must have
read-and-write access (a read-only key returns `403`). Base URL: `https://open-api.getcakewalk.io/api/v1`.

## Steps
1. **Find the work app.** `GET /WorkApps` and locate the target app's `workAppId`
   (paginate with `links.next` if needed).
2. **Check permission levels.** `GET /WorkApps/{workAppId}/PermissionLevels`. If the app has
   at least one permission level, a `permissionLevelId` is **mandatory** in the next step.
3. **Create the grant request.** `POST /Requests/GrantAccess` with body:
   - `createdByUserId` (UUID, required) — user shown as the request creator
   - `targetUserId` (UUID, required) — user who will receive access
   - `workAppId` (UUID, required) — the app from step 1
   - `grantAccessPurpose` (string 1–1024, required) — reason for the grant
   - `permissionLevelId` (UUID, optional/conditional) — required when the app has permission levels
   On success you get `201 Created` with a `requestId`.
4. **Track it.** `GET /Requests` to confirm the request and watch its status as approval
   policies route it to approvers.

## Rules
- Errors use RFC 7807 ProblemDetails (`type`, `title`, `status`, `detail`, `instance`).
  Handle `400` (validation), `401` (bad credentials), `403` (read-only key / no permission),
  `409` (conflicting/duplicate request).
- Respect the 1,000 requests-per-minute rate limit; back off on `429`.
- The request is not idempotent — do not blindly retry a `POST /Requests/GrantAccess` that may
  have already succeeded; re-check with `GET /Requests` first.
