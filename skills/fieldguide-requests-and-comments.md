---
name: Work Fieldguide requests and comments
description: Track client requests (PBC items) on an engagement, attach files, and collaborate via comments.
api: openapi/fieldguide-openapi-original.json
operations:
  - list_requests_by_engagement_v1
  - get_request_v1
  - update_request_v1
  - list_request_files_v1
  - upload_request_file_v1
  - list_comments_by_request_v1
  - create_request_comment_v1
---

# Work Fieldguide requests and comments

## Auth
- `Authorization: Bearer <token>`. Needs `requests:read`/`requests:write`, `requests.files:read`/`requests.files:write`, and `comments:read`/`comments:write` as appropriate.

## Steps
1. **List requests on an engagement** — `list_requests_by_engagement_v1` (`GET /v1/engagements/{uuid}/requests`). Filter with `is_completed`, `due_date_before`, `statuses`.
2. **Inspect a request** — `get_request_v1` (`GET /v1/requests/{uuid}`).
3. **Update status/fields** — `update_request_v1` (`PATCH /v1/requests/{uuid}`).
4. **Attach evidence** — `list_request_files_v1` (`GET /v1/requests/{uuid}/files`) then `upload_request_file_v1` (`POST /v1/requests/{uuid}/files`).
5. **Collaborate** — `list_comments_by_request_v1` (`GET /v1/requests/{uuid}/comments`) and `create_request_comment_v1` (`POST /v1/requests/{uuid}/comments`).

## Rules
- All ids are UUIDs; handle `403` (name the missing scope), `404`, and `429`.
- To react to request/comment changes without polling, subscribe to webhooks (see the Fieldguide webhooks skill).
