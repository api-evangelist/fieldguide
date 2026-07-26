---
name: Manage Fieldguide engagements
description: List, inspect, create-from-template, and export audit/advisory engagements in Fieldguide.
api: openapi/fieldguide-openapi-original.json
operations:
  - list_engagements_v1
  - get_engagement_v1
  - update_engagement_v1
  - create_engagement_from_template_v1
  - export_engagement_v1
  - get_engagement_export_v1
  - get_job_v1
---

# Manage Fieldguide engagements

Use the Fieldguide REST API (`https://api.fieldguide.io`) to work with engagements.

## Auth
- Send `Authorization: Bearer <token>` on every request (JWT / API token).
- Requires the `engagements:read` scope for reads and `engagements:write` for writes. A missing scope returns `403` naming the required scope.

## Steps
1. **Find the engagement** — `list_engagements_v1` (`GET /v1/engagements`). Page with `page` and `per_page`; filter with `statuses`, `templates`, `engagement_lead_user_uuid`.
2. **Inspect it** — `get_engagement_v1` (`GET /v1/engagements/{uuid}`).
3. **Update it** — `update_engagement_v1` (`PATCH /v1/engagements/{uuid}`) with the fields to change.
4. **Create from a template** — `create_engagement_from_template_v1` (`POST /v1/engagements/from-template`). This is accepted for async processing (`202`) and returns a job.
5. **Export a binder** — `export_engagement_v1` (`POST /v1/engagements/{uuid}/export`) returns a job UUID (`202`); poll `get_engagement_export_v1` (`GET /v1/engagements/exports/{uuid}`) or `get_job_v1` (`GET /v1/jobs/{uuid}`) until complete (a `302` redirects to the finished artifact).

## Rules
- Resources are addressed by UUID. Handle `404` (not found / no access) and `429` (rate limited — back off and retry).
- Long-running operations (create-from-template, export) are asynchronous jobs; always poll to completion.
