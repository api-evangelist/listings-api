---
name: listings-api-respond-to-reviews
description: Read, triage, and publicly reply to customer reviews across a Listings API account, safely.
api: Listings API
generated: '2026-09-03'
method: generated
source: openapi/listings-api-openapi.yaml, https://docs.listingsapi.com/docs/reviews, https://docs.listingsapi.com/docs/error-codes
operations: [rollupInteractions, interactions, interactionDetails, interactionSiteConfig, respondToInteraction, editReviewResponse, archiveReviewResponse, interactionsAnalyticsStats]
---

# Respond to reviews with the Listings API

Replies published through this API go LIVE on the review site. Treat every write as public.

## Steps

1. **Find locations needing attention.** `GET /api/v4/rollup_interactions` (`rollupInteractions`)
   rolls review counts up across the account. For one location,
   `GET /api/v4/locations/{locationId}/reviews` (`interactions`) lists reviews with rating,
   author, text, date, and source site. Paginate with `first`/`after` using
   `pageInfo.endCursor` while `hasNextPage` is true.
2. **Check whether you can reply.** Replies work on Google and Facebook only; Yelp is
   read-only. Check the review's `canRespond` field (and `GET /api/v4/reviews/site-config`,
   `interactionSiteConfig`, for per-site capabilities) before drafting.
3. **Fetch the exact review.** `GET /api/v4/reviewDetails` (`interactionDetails`) by ID when
   you need the full record.
4. **Publish the reply.** `POST /api/v4/locations/reviews/respond` (`respondToInteraction`).
   The transport can return HTTP 200 while the write fails: ALWAYS read
   `data.respondToInteraction.errors[]` — a populated array means the reply was rejected
   (SY-prefixed message names the rule).
5. **Fix or withdraw.** `POST /api/v4/locations/reviews/respond/edit` (`editReviewResponse`)
   revises a live reply; `POST /api/v4/locations/reviews/respond/archive`
   (`archiveReviewResponse`) soft-deletes it from the local cache.
6. **Measure.** `GET /api/v4/locations/{locationId}/review-analytics-overview`
   (`interactionsAnalyticsStats`) for the reputation overview.

## Guardrails

- No idempotency mechanism exists: never blind-retry a `respondToInteraction` after a timeout —
  re-read the review's responses first, or you may post a duplicate public reply.
- An empty analytics or reply failure usually means the Google/Facebook profile is not
  connected/matched to the location (see the connected-accounts operations).
- 429: honor `retry_after_seconds` from the error body (Launch is 10 req/min per account).
