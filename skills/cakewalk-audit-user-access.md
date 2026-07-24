---
name: Audit which apps a user can access
description: Look up a user and enumerate the apps and accesses they hold for an access review or audit.
api: https://open-api.getcakewalk.io/api/v1
operations:
  - GET /users
  - GET /users/{id}
  - GET /WorkApps
  - GET /WorkApps/{workAppId}/Accesses
generated: '2026-07-18'
method: generated
source: https://www.cakewalk.security/docs/open-api-and-mcp/api-reference
---

# Audit which apps a user can access

Use this skill to build an access picture for a user — which apps they can reach and at what level
— to support an access review or audit.

## Authentication
Send `X-API-KEY` and `X-API-SECRET` headers. A read-only key is sufficient for this read-only flow.
Base URL: `https://open-api.getcakewalk.io/api/v1`.

## Steps
1. **Resolve the user.** `GET /users` to find the user, or `GET /users/{id}` when you already have
   the id. Paginate with `links.next`.
2. **Enumerate work apps.** `GET /WorkApps` to list apps in scope (paginate).
3. **Read each app's accesses.** For each relevant app, `GET /WorkApps/{workAppId}/Accesses` and
   filter to the target user to see whether and at what permission level they have access.
4. **Compile.** Assemble the user's app + permission-level footprint for the review record.

## Rules
- Follow `links.next` for cursor pagination; never construct cursor values manually.
- Errors are RFC 7807 ProblemDetails; handle `401`, `403`, `404`.
- Respect the 1,000 requests-per-minute rate limit; back off on `429`.
- For a bulk audit, prefer the MCP tools `ListUsersByWorkAppIds` and `ListUserAccessesByEmails`
  (see mcp/cakewalk-mcp.yml) which answer these questions in one call.
