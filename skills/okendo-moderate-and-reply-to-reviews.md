---
name: Moderate and reply to Okendo reviews
description: List pending product reviews, approve or reject them, and post a merchant reply.
api: Okendo Merchant REST API
base_url: https://api.okendo.io/enterprise
operations: [listReviews, getReview, updateReview, replyToReview]
---

# Moderate and reply to Okendo reviews

Use the Okendo Merchant (Enterprise) REST API to triage the review moderation queue.

## Authentication
- HTTP Basic: `Authorization: Basic base64("<okendo_user_id>:<api_key>")` (credentials from Okendo app > integration settings).
- Always send `okendo-api-version: 2025-02-01`.
- This API is server-side only — never call it from the browser.

## Steps
1. **List the pending queue** — `GET /reviews?status=pending&limit=100&orderBy=date desc` (`listReviews`). Page with `lastEvaluated` / follow `nextUrl` until exhausted.
2. **Inspect a review** — `GET /reviews/{reviewId}` (`getReview`) to read the full body, rating and media before deciding.
3. **Set moderation status** — `PUT /reviews/{reviewId}` (`updateReview`) to move it to `approved` or `rejected`.
4. **Reply publicly** — for approved reviews needing a response, `POST /reviews/{reviewId}/reply` (`replyToReview`).

## Conventions
- Pagination is cursor-based: `limit` (1-100, default 25), `lastEvaluated` cursor, `nextUrl` in the response.
- `status` filter enum: `approved` | `pending` | `rejected` (default `approved`).
- No idempotency key is documented — treat `updateReview`/`replyToReview` as non-idempotent and avoid blind retries.
