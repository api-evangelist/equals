---
name: Manage Equals analyst memories
description: >-
  Read, add, correct and remove the facts the Equals AI analyst remembers about a workspace, using the
  Equals Memories API — for syncing definitions from another system of record, auditing what the
  analyst knows before trusting a number, or cleaning up stale context.
api: openapi/equals-memories-openapi.yml
operations:
  - listMemories
  - getMemory
  - createMemory
  - updateMemory
  - deleteMemory
generated: '2026-08-14'
method: generated
source: https://docs.equals.com/docs/memories-api.md
---

# Manage Equals analyst memories

A **memory** is one plain-text fact the Equals AI analyst carries into every question it answers about a
workspace — a metric definition, a fiscal calendar, an exclusion rule. Memories are workspace-scoped, so
a token from workspace A can never see or change workspace B's context.

## Before you start

- Get an API token: **Settings → API tokens → Create token** in Equals. The token is displayed **once,
  at creation**. It acts as the user who created it and is scoped to that user's workspace.
- Base URL: `https://go.equals.com/api/v1`
- Every request carries `Authorization: Bearer <token>`. There is no anonymous access — an unauthenticated
  call returns `401 {"error":"Unauthorized"}`.

## Steps

1. **Audit what the analyst already knows.** Call `listMemories` (`GET /memories`). It returns
   `{"memories":[...]}`, newest first. There is no documented pagination — read the whole array.
2. **Read one memory** with `getMemory` (`GET /memories/{id}`) when you are reconciling against an
   external record and only hold the id.
3. **Add a fact** with `createMemory` (`POST /memories`) and a body of `{"content": "..."}`. Success is
   `201` plus the created object, including its server-assigned `id`. Write one fact per memory — the
   analyst retrieves them individually, so a paragraph containing five rules retrieves as one blunt unit.
4. **Correct a fact** with `updateMemory` (`PATCH /memories/{id}`) and `{"content": "..."}`. `content` is
   a **replacement**, not a merge: send the full corrected sentence, not a diff.
5. **Remove a fact** with `deleteMemory` (`DELETE /memories/{id}`). Success is `204` with an empty body —
   do not try to parse the response.

## Rules an agent must follow

- **No idempotency key exists.** Equals documents no `Idempotency-Key` header, so `createMemory` is
  **not** safe to blind-retry: a retried create makes a second memory with the same content and a new id.
  On a timeout, call `listMemories` and match on `content` before retrying.
- **Errors use a bare envelope, not RFC 9457.** Every failure is `{"error": "<message>"}` with
  `Content-Type: application/json`. Branch on the status code, not the string:
  `401` bad/revoked/expired token · `403` the token's user left the workspace · `404` no such memory in
  this workspace · `422` invalid body, e.g. empty `content`.
- **`403` is not retryable** — it means membership was removed. Stop and surface it to a human.
- **No rate limits are published** and no `RateLimit-*` or `Retry-After` headers were observed. Treat the
  API as best-effort: serialize writes and back off on any non-2xx rather than assuming headroom.
- **Deleting is destructive and unversioned.** There is no undo and no soft-delete. Read before you
  delete, and prefer `updateMemory` over delete-then-create so the id stays stable for external mappings.
- **Treat memory content as durable instruction.** Anything written here steers every future answer the
  analyst gives. Do not write speculative or user-supplied text into a memory without review.

## Related surfaces

- The hosted Equals MCP server (`https://go.equals.com/api/mcp`, OAuth) is the *querying* surface —
  workbook discovery, datasource SQL, and natural-language analysis. Memories are the *context* the
  analyst applies while doing that. See `mcp/equals-mcp.yml`.
- Conventions, error envelope and auth details: `conventions/equals-conventions.yml`,
  `errors/equals-problem-types.yml`, `authentication/equals-authentication.yml`.
