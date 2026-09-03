---
name: listings-api-publish-local-posts
description: Publish announcements, events, and offers to Google Business Profile and Facebook across one or many locations.
api: Listings API
generated: '2026-09-03'
method: generated
source: openapi/listings-api-openapi.yaml, https://docs.listingsapi.com/docs/posts
operations: [createSocialPost, createBulkSocialPost, socialPostView, socialPostViewBulk, rollupSocialPostsByLocation, rollupSocialPostsBulk, deleteSocialPost]
---

# Publish local posts with the Listings API

Posts go live on connected Google Business Profile and Facebook pages. There is no in-place
edit: to change a live post, delete and repost.

## Steps

1. **One location:** `POST /api/v4/posts` (`createSocialPost`) with the post type
   (announcement, event, or offer). **Many locations:** `POST /api/v4/bulk-posts`
   (`createBulkSocialPost`) fans one post across locations in a single call.
2. **Read back per-publisher results.** `GET /api/v4/bulk-posts/{postId}`
   (`socialPostViewBulk`) reports the per-location, per-publisher outcome;
   `GET /api/v4/posts/{postId}` (`socialPostView`) for a single post.
3. **List what is live.** `GET /api/v4/locations/{locationId}/posts`
   (`rollupSocialPostsByLocation`) and `GET /api/v4/locations/{locationId}/bulk-posts`
   (`rollupSocialPostsBulk`).
4. **Take a post down.** `DELETE /api/v4/posts/{postId}` (`deleteSocialPost`).

## Guardrails

- Channels are Google and Facebook only — do not promise Instagram/X delivery from this surface.
- Mutations can fail on HTTP 200: read `data.<operation>.errors[]` on every create.
- The publishing location needs its Google/Facebook profile connected and matched first.
- No idempotency mechanism: a retried create can publish the same post twice. Check
  `rollupSocialPostsByLocation` before re-issuing a create after a timeout.
- Bulk fan-out on the Launch plan (10 req/min) needs batching; honor `retry_after_seconds` on 429.
