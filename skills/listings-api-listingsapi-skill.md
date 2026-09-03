---
name: listingsapi
description: Create, sync, and manage local business listings, reviews, posts, and local analytics through the Listings API REST API. Use when a task involves publishing a business to Google, Facebook, Bing, Apple Maps, or local directories, reading or replying to reviews, publishing posts, or reading local search analytics.
---

# Listings API

Listings API is a REST API for local SEO. One write publishes a business location to every citation on its plan, and the same key covers reviews, posts, and analytics.

## Authentication

Every request carries one header:

```
Authorization: API <your-api-key>
```

Get a key by signing up at https://listingsapi.com/signup. Keys are managed in the dashboard at https://listingsapi.com/dashboard. Issue a separate key per agent so each can be revoked on its own.

Base URL: `https://listingsapi.com`

## Core endpoints

- `POST /api/v4/locations` creates a location and queues it for publishing across the plan's network.
- `POST /api/v4/locations/update` updates fields on an existing location; the change re syncs every citation.
- `GET /api/v4/locations/{id}/listings/premium` returns one record per citation with syncStatus, displayStatus, actionRequired, syncIssue, and the live listingUrl.
- `GET /api/v4/locations/{locationId}/reviews` lists reviews; `POST /api/v4/locations/reviews/respond` posts a public owner reply (Google and Facebook).
- `POST /api/v4/posts` publishes an announcement, event, or offer to Google and Facebook; `POST /api/v4/bulk-posts` fans one post across many locations.
- `GET /api/v4/locations/{id}/google-analytics`, `bing-analytics`, and `facebook-analytics` return per location performance as JSON.

## References

- OpenAPI spec: https://listingsapi.com/openapi.yaml
- Documentation: https://docs.listingsapi.com/docs
- MCP server (streamable HTTP, same Authorization header): https://listingsapi.com/mcp
- Plain text site index for agents: https://www.listingsapi.com/llms.txt
