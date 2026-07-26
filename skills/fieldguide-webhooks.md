---
name: Subscribe to Fieldguide webhooks
description: Create and manage outbound webhook subscriptions for company/engagement/request/comment/user lifecycle events.
api: openapi/fieldguide-openapi-original.json
operations:
  - list_webhooks_v1
  - create_webhook_v1
  - update_webhook_v1
  - delete_webhook_v1
---

# Subscribe to Fieldguide webhooks

Fieldguide POSTs a signed JSON event to your URL when a subscribed object is created, updated, or deleted.

## Auth
- `Authorization: Bearer <token>` with `webhooks:read` / `webhooks:write`.

## Steps
1. **Create a subscription** — `create_webhook_v1` (`POST /v1/webhooks`) with `description`, `url`, and optional `scopes` (any of `comments`, `companies`, `engagements`, `requests`, `users`; omit/`null` to receive all). The response returns the signing `secret` once — store it.
2. **List subscriptions** — `list_webhooks_v1` (`GET /v1/webhooks`).
3. **Update** — `update_webhook_v1` (`PATCH /v1/webhooks/{uuid}`) to change URL or scopes.
4. **Delete** — `delete_webhook_v1` (`DELETE /v1/webhooks/{uuid}`).

## Handling deliveries
- Each event payload has: `uuid` (event id), `timestamp`, `action` (`created`|`updated`|`deleted`), `object_type` (`comments`|`companies`|`engagements`|`requests`|`users`), `object_uuid`, and `metadata[]` (e.g. `company_uuid`).
- Verify the signature using the stored `secret`. Respond `2xx` quickly and process asynchronously.
- See `asyncapi/fieldguide-webhooks.yml` for the full event catalog.
